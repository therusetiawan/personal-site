---
title: "How to Deal With Laravel Task Scheduling on Heroku"
date: 2018-12-05T20:41:58+07:00
draft: false
categories: ["Tech"]
tags: ["Laravel","Heroku"]
---

Three months ago, i was assigned to one of [Qiscus](https://qiscus.com/) product
called [Qismo](https://qismo.qiscus.com/). Qismo is an accelerator package as a
solution for customers service. It is an integrated service which connects
customers to merchants via currently available messaging services in the market
such as FB messenger, Line, Telegram and Qiscus widget. This allows a merchant
to gather all incoming messages from customers in one place and get back to them
in no time.

As a SaaS (Software as a Service) product, Billing & Payment is an important
feature. In the other hand, currently Qismo already have so many clients. We
need to filter who is paid clients and who is free clients. And we want to make
clients can make a payment via Qismo dashboard based on their plan subscription.
So, my team plan to ship Billing & Payment feature in the end of October 2018.
Then we start to gather the requirements with Qiscus Business and Operations
Team. Here are the list of functionalities that should we build :

* Qismo will have list of plans (Starter, Business, Enterprise, Custom)
* Qismo will have monthly and annually subscription type
* Qismo user will have 14 days free trial
* After 14 trial days, free account will be disabled automatically
* Qismo will create an invoice to paid account automatically based on their plan
(recurring invoices)
* Qismo will send email notifications automatically on some events such as
disabled free account, new invoice, invoice due date and qismo account monthly
usage
* Qismo will support bank transfer payment method

Based on the list above, there are some functionalities that should be run
automatically. For example disable free account, send email notifications and
create recurring invoices. To develop that features, we choose to use Laravel
Task Scheduling.

This laravel feature allow you to fluently and expressively define your command
schedule within Laravel itself. When using the scheduler, only a single Cron
entry is needed on your server. In the other words, you don’t need to generated
a Cron entry for each task you needed to schedule. Yes, its so simple and funny.

Let’s focus on Qismo use cases again. Our scenario is Qismo will have several
background jobs to handle Billing & Payment feature. Here is the background job
sample.

```
<?php

    namespace App\Jobs\Payment;

    use Illuminate\Bus\Queueable;
    use Illuminate\Queue\SerializesModels;
    use Illuminate\Queue\InteractsWithQueue;
    use Illuminate\Contracts\Queue\ShouldQueue;
    use Illuminate\Foundation\Bus\Dispatchable;
    ........

    class DisableFreeAcountJob implements ShouldQueue
    {
        public function __construct()
        {
            
        }

        public function handle()
        {
            // Code to disable qismo free account

        }

    }
```

Basically, another background jobs are same with `DisabledFreeAcountJob` above.
The main different only on the business logic on each background jobs. All
background jobs are placed in folder `App\Models\Jobs\Payment`.

```
    ├── app
    │   ├── Console
    │   ├── Exceptions
    │   ├── Http
    │   ├── Jobs
            ├── Payment
            │   │   ├── DisabledFreeAcountJob.php
            │   │   ├── CreateRecurringInvoicesJob.php
            │   │   ├── SendEmailNotificationsJob.php
    │   ├── Mail
    │   ├── Models
    │   ├── Modules
    │   ├── Providers
    │   ├── Scopes
    ├── bootstrap
    ├── config
    ├── routes
    ├── ..............
```

Then we want to run all payment background jobs daily. It’s very simple. We just
need to add all scheduled tasks in the `schedule` method of the
`App\Console\Kernel.php`. Here is the details.

```
<?php

namespace App\Console;

use DB;
use Illuminate\Console\Scheduling\Schedule;
use Illuminate\Foundation\Console\Kernel as ConsoleKernel;

class Kernel extends ConsoleKernel
{
    .....................
protected function schedule(Schedule $schedule)
    {
        $schedule->job(new \App\Jobs\Payment\DisableFreeAcountJob())->dailyAt('01:00');
        $schedule->job(new \App\Jobs\Payment\CreateRecurringInvoicesJob())->dailyAt('01:00');
        $schedule->job(new \App\Jobs\Payment\SendEmailNotificationsJob())->dailyAt('01:00');
   }
}
```

Now we can run all background jobs above only using one command. The command is
`php artisan schedule:run`. Next step is we need to add Heroku plugin called
`Heroku Scheduler`. This plugin will execute `php artisan schedule:run` command
periodically and allows you to configure jobs to run every 10 minutes, every
hour, or every day, at a specified time.

For Qismo use case, we choose to exucute `php artisan schedule:run` daily every
01:00 UTC. From the image below, we can monitor what is the next due and what is
the last run.

![Heroku Scheduler](/img/heroku-scheduler-1.png)

#### The problem

But the reality, next due is not accurate. Sometime next due execution is late.
This condition will affected to all payment background jobs that never be
executed. To be honest, i was struggling with this issue around 2 days.

![Heroku Scheduler](/img/heroku-scheduler-2.png)


#### **So what is the solution?**

In order to handle the problem above, we need to do extra codes. There are 3
steps : create crons table, create the model and run the task based on a truth
tests.

**1 — Create migration to create crons table**

Crons table have 3 columns. First, command column is used to store background
job name that will be executed. Next_run & last_run column is same with next_due
and last_run on `Heroku Scheduler` configuration. Command column is a primary
key, so the value must be unique. It’s to prevent command duplication.
```
<?php
use Illuminate\Support\Facades\Schema;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Database\Migrations\Migration;
use Carbon\Carbon;
class CreateCronsTable extends Migration
{

    public function up()
    {
        Schema::create('crons', function (Blueprint $table) {
            $table->string('command');
            $table->datetime('next_run');
            $table->datetime('last_run');
            $table->timestamps();
            $table->primary('command');
        });
    }
    public function down()
    {
        Schema::dropIfExists('crons');
    }
}
```
After run the migration to create crons table above, don’t forget to insert list of payment background jobs manually. So, crons table contains 3 records.

![List of Jobs](/img/list-of-jobs.png)

**2 — Create the model**
```
<?php

    namespace App\Models;

    use Illuminate\Database\Eloquent\Model;
    use Carbon\Carbon;

    class Cron extends Model
    {
        protected $primaryKey = 'command';

        protected $fillable = ['command', 'next_run', 'last_run'];

        public static function shouldRun($command, $days) {
            $cron = Cron::find($command);
            $now  = Carbon::now();

           if ( empty($cron) ||
                date('Y-m-d H:i', strtotime($cron->next_run)) > date('Y-m-d H:i', strtotime($now))
            ) {
                return false;
            }

           $cron->next_run = Carbon::parse($cron->next_run)->addDays($days);
            $cron->last_run = Carbon::now();
            $cron->save();

            return true;
        }
    }
```

Cron model only contain one static function to check a command should be run or not. A command should be run if command is found in database and the next run is less than the current time.

**3 — Run scheduler based on the truth tests**

```
$schedule->job(new JOBNAME())->everyMinute()->when(function() {
          return Cron::shouldRun('JOBNAME()', 1);
    });
```
Update laravel task scheduling configuration in `App\Console\Kernel.php` using
the truth tests above. Please replace JOBNAME() with three payment background
jobs name.
