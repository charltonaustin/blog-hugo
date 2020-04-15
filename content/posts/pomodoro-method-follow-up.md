---
title: Pomodoro Method Follow Up
date: 
draft: true
description: "How to amp up your desktop pomodoro game."
tags: [
    "pomodoro",
]
categories : [
    "management",
]
---
## Pomodoro Technique
I've blogged about it before.
Recently I dusted off my old method and tried to implement it.
I found that there were some rather strange problems with my old method and MacOS.
So I wanted to help some people.

## The scripts
We have a few scripts.
We have a [delayed_mac](https://github.com/charltonaustin/dotfiles/blob/master/bin/delayed_mac) script.
This script just takes a command and a time in mins.
It executes the command that many mins later.
Next we have a [take_a_break](https://github.com/charltonaustin/dotfiles/blob/master/bin/take_a_break) script.
It sets a timer a timer for 5 or 15 mins later depending on your choice.
It will run the pomondoro script after time is up.
Next we have a [pomodoro](https://github.com/charltonaustin/dotfiles/blob/master/bin/pomodoro) script.
It asks you what your task is and then sets sets up to run the break script 25 mins later.
All of this is written to a file which is gets rolled up with a timestamp.
The scripts use [atrun](https://www.freebsd.org/cgi/man.cgi?query=atrun&sektion=8) to schedule the tasks.


## Upgrades
Finally I added a new feature.
The scripts now play an alarm.
This way I know when it is time to get back to work.
You can see the script [here](https://github.com/charltonaustin/dotfiles/blob/master/bin/play_alarm)

## Gotcha!!
One problem I had with the script was it would silently fail.
Mac recently updated it's plist so that they are binary.
I noticed the change when I moved from Mojave to Catalina.
By default the at run plist located at /System/Library/LaunchDaemons/com.apple.atrun.plist is turned off.
Before you could modify the file by turning off system integrity process.
But with the new binary I couldn't even get that to work.
In the end I had to do two things.
First you have to enable the atrun background process overriding the config.
You can simply run this command.

`launchctl load -Fw /System/Library/LaunchDaemons/com.apple.atrun.plist`

Next you have to update the Security and Privacy settings in System Settings.
In particular you have to give atrun and your terminal program full disk access.
