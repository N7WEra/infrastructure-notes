---
description: Different type of terminals and shells
---

# Terminals

## tmux

**What is tmux?** 

tmux  is like screen + terminator combined, The biggest benefit for tmux is  that it run the terminal as a process and not bind to the session,  sessions won’t die if you were disconnected. 

**How to install?** 

tmux is build in into Kali, but if not you can install it as ‘apt install tmux’ 

**Starting tmux** 

For every engagements it’s recommended to create new session which will be for that engagement, by typing: 

`tmux new –s %NAME%` 

\(replace name with the name of engagement\).Notice  on the bottom terminal there is a green line with all the information  on each window. \(where there is a star next to the name it mean you  currently viewing this window\)Each windows have a number and a name, the name can be changed.For each command we will need to press the prefix key \(which is by default ctrl +b\) first. 

**Simple commands:** 

To start a new windows in the terminal press prefix \(ctrl +b\) + c. 

To move between windows press the prefix and the number of the window you want to move to. 

Change windows name: Prefix + , 

Rename the session: Prefix + $ 

**Attach, Detach and Kill:** 

To  exit the tmux session press prefix + d – and it will detach the current  session and return to your normal shell \(this won’t kill your session\) 

To  re-attach the session write ‘tmux attach-session’ or ‘tmux a’ \( we can  even create multiple sessions and attach based on numbers\)You can view all the current sessions by running ‘tmux ls’ 

**Tmux configuration file** 

You can customise the tmux configuration by creating a 

~/.tmux.conf 

tmux will pick this configuration file for your current user 

In the configuration file you can change the prefix key, extend the scroll back \(history\) 

set -g history-limit 10000 \(extend the history\) 

set -g allow-rename off \(stop from the session to rename the windows automatically\) 

**Splitting screens** 

Prefix + % - split the screen vertically 

Prefix + “  -split the screen horizontally 

Prefix + &lt;arrow key&gt; to move between the screens 

Prefix + Z – to zoom into one screen \(and also to exit the screen from zoom\) 

Prefix + { - replace the screen location to the left 

Prefix + } – replace the screen location to the right 

Prefix + &lt;Space&gt; -  change layout 

**Few other tricks:** 

Press ctrl +a to go to the beginning of the line 

Press ctrl +e to get to the end of the line 

Press ctrl and a arrow key to go word by word 

Prefix+ t  - show the time 

**To Learn more:** 

{% embed url="https://www.youtube.com/watch?v=Lqehvpe\_djs" %}

## Byobu

**What is Byobu?** 

Byobu is a GPLv3 open source text-based window manager and terminal multiplexer. It was originally designed to provide elegant enhancements to the otherwise functional, plain, practical GNU Screen, for the Ubuntu server distribution. Byobu now includes an enhanced profiles, convenient keybindings, configuration utilities, and toggle-able system status notifications for both the GNU Screen window manager and the more modern Tmux terminal multiplexer, and works on most Linux, BSD, and Mac distributions. 

**How to install?** 

On Ubuntu/Kali: 

`apt install byobu` 

**Starting Byobu:** 

To start byobu write in a terminal: 

`Byobu` 

**Simple commands:** 

Shift + F1 - show help menu 

F2 - to create a new window 

F3 - move to the left window 

F4 - move to the right window 

Shift + F2 - split pane horizontally 

Ctrl + F2 - split pane vertically 

Shift + Up/Down/Left/Right - move between splits 

Ctrl + F3/F4 - move splits around 

F8 - rename window 

Shift-Alt-Left/Up/Down/Right - to resize split window 

Ctrl - F6 - kill split in focus 

Alt + PageUp/PageDown - scroll in long text \(in support vi commands - use '/' to search 

Shift- F11 - zoom into a pane \(and to restore\) 

**Few other tricks:** 

Ctrl+Shift + F2 - create new session 

F9 - change the bottom line information options 

Alt + F11 - take a pane out of split into a new window 

Ctrl + F11 - join 2 windows together to a split screen 

To Learn more: 

[http://byobu.co/](http://byobu.co/) 

{% embed url="https://www.youtube.com/watch?v=NawuGmcvKus" %}



