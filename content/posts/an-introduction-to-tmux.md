---
title: "An Introduction to Tmux"
date: 2018-09-21T21:38:29+07:00
draft: false
categories: ["Tech"]
tags: ["Terminal", "Tmux"]
---

#### What is Tmux?
Tmux is a terminal multiplexer. It's allow you to open multiple terminal sessions inside a single terminal window and easily switching between them. Tmux also allow you to split terminal into multiple windows. Means, you don't need to open multiple tabs in terminal when you have multiple task. Trust me, tmux is very effective and will save your time!

#### Session, Window and Pane
When you use tmux, terminal tab is definitely deprecated. It's replaced by window. Let's me explains about session, window and pane one by one. Session is tmux main feature. When you start using tmux, it will create new session. A session can contains multiple window. You can assume that window is same with terminal tab. And the last one is pane. A window can be split into multiple part, both vertically or horizontally.

![Tmux Basic] (/img/tmux-introduction.png)

#### Basic Usage
```
Start using tmux
$ tmux new

Start using tmux with named session
$ tmux new -s [session name]

List of tmux session
$ tmux ls

Attach to named session
tmux a -t [name of session]

Kill tmux session
$ tmux kill-session -t [session name]

Create new window
Ctrl+b c

Create new horizontal pane
Ctrl+b %

Create new vertical pane
Ctrl+b "
```

See tmux chathseet on this link https://gist.github.com/therusetiawan/7dcc2fbece9353de7dd14bb461bc5df3

#### Pair Programming with Tmux
Pair programming is when two programmers work together and share one screen. The easiest way to do that is two programmers sit down side by side and use one computer. But the problem is when two programmer working remotely, so its impossible to sit down side by side.

Yes, tmux is one of the best solution for the problem above. It's very easy to do pair programming with tmux. No need to install additional software on your computer. Programmers only need to join in the same tmux session and viola they can see same screen. And the good news is tmux can work with slow internet connection. Hmm that looks amazing.

![Pair Programming with Tmux] (/img/tmux-pair-programming.gif)

In the end of this post, i want to share my tmux configuration that stored in my github repository https://github.com/therusetiawan/dotfiles/blob/master/.tmux.conf. Feel free to contact me if you have any questions about my tmux configuration.
