---
title: "How to Save Tmux Session"
date: 2021-01-01T08:23:15+07:00
draft: false
categories: ["Tech"]
tags: ["Terminal", "Tmux", "Window"]
---

Hi folks,

When you start using tmux, it will always create a new session. We can say that session is tmux main feature. Session can contains multiple windows and panes. It depend on your needed.

The interesting thing is you can create multiple tmux sessions. Usually, every session only related to one project. For example you have 2 tmux session. The first is “laravel” session contains 2 panes. First pane is used to open vim editor and the second pane is used to run the laravel app.

![Tmux session contains 2 panes] (/img/save-tmux-session-1.png)

Another tmux session named “hugo” contains 3 panes. First pane is used to open vim. Second pane is used to run hugo server. And the last pane is used to run git command.

![Tmux session contains 3 panes] (/img/save-tmux-session-2.png)

By using the method above, you can easily switch to another project. Switching tmux session also mean switch to another project. It will be harder if you open two project using text editor/IDE. You should switch to another window to goto another project. This situation will be more complicated if you open so many project. I think it will be affected to your computer performance because many windows need many resource too.

But, tmux still have a negative side. When you shutdown the computer, tmux is not save session/window/pane. Means, every time you open tmux you should configure session/window/pane again. It will be very very painful activities.

The solution is adding [tmux resurrect plugin] (https://github.com/tmux-plugins/tmux-resurrect). This plugin saves all the little details from your tmux environment so it can be completely restored after a computer restart. No configuration is required. You should feel like you never quit tmux.

Using tmux resurrect is very easy. There are two key bindings. prefix + Ctrl-s to save tmux environment and prefix + Ctrl-s to restore tmux environment. It's as simple as that.

You can still enhanced how to save tmux environment by adding [tmux-continum plugin] (https://github.com/tmux-plugins/tmux-continuum). Tmux-continum continuously save tmux environment every 15 minutes. All the saving happens in the background without the impact to your workflow. This action starts automatically when the plugin is installed.

