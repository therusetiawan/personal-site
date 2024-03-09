---
title: "How to Deal With Tmux Session"
date: 2018-09-29T19:04:46+07:00
draft: false
categories: ["Tech"]
tags: ["Terminal", "Tmux"]
---

Hi folks,

When you start using tmux, a new session will always appear. We can say that a
session is tmux main feature. A session can contain multiple windows and panes.
It depend on your need.

The interesting thing is you can create multiple tmux sessions. Usually, every
session only related to one project. For example you have 2 tmux sessions.
Session “laravel” contains 2 panes. The first pane is used to open vim editor
and the second pane is used to run the laravel app.

![Tmux session named "laravel"] (/img/laravel-session.png)

Another tmux session named “rails” contains 3 panes. First pane is used to open
vim. Second pane is used to run the rails app. And the last pane is used to run
rails console command.

![Tmux session named "rails"] (/img/rails-session.png)

By using the method above, you can easily switch to another project. Switching
tmux session also means switch to another project. It will be difficult if you
open two projects using text editor/IDE. You should switch to another window to
go to another project. This situation will be more complicated if you open so
many projects. I think it will be affected to your computer performance because
many windows need many resources too.

On the other hand, tmux still have a negative side. When you shut down the
computer, tmux is not saving session/window/pane. It means, every time you open
tmux you should configure session/window/pane again. It is fatiguing activities.

The solution is adding [tmux-resurrect] (https://github.com/tmux-plugins/tmux-resurrect) 
plugin. This plugin saves all the little
details from your tmux environment so it can be completely restored after a
computer restart. No configuration is required. You should feel like you never
quit tmux.

Using tmux resurrect is very easy. There are two key bindings. Key binding
`prefix + Ctrl-s`is used to save tmux environment and `prefix + Ctrl-s` is used
toto restore tmux environment. It's as simple as that.

You can still enhance how to save tmux environment by adding [tmux-continum] (https://github.com/tmux-plugins/tmux-continuum)
plugin. Tmux-continum continuously saves tmux environment every 15 minutes. All
the saving happens in the background without the impact to your workflow. This
action starts automatically when the plugin is installed.
