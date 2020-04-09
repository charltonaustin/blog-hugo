---
title: Pomodoro On Os X Updated
date: 2018-03-29T09:03:54-04:00
draft: false
---
### Pomodoro Expanded
Previously I had created [a post](/2016/11/03/pomodoro-on-os-x) where I talked about using the pomodoro technique on mac OSX. 
Well today I want to show how I have expanded on my original idea.
First I created this:
```
#! /bin/sh
at now + $1 minutes <<< $2
```
Which is a small script to run a bash command at some time in the future.
Then I created this:
```
#!/bin/bash
x=`/usr/bin/osascript <<EOT
tell application "Finder"
    activate
    set myReply to text returned of (display dialog "Start 25 mins: Please put in task" default button 1 buttons {"OK"} default answer "" with icon 1)
end tell
EOT`
echo 'Text is: '$x
if [[ -z "$x" ]]; then
  echo "Done for the day"
else
  mkdir -p $HOME/work
  touch $HOME/work/daily_pomodoro.txt
  echo `date` $x >> $HOME/work/daily_pomodoro.txt
  delayed_mac 25 five_min
fi
```
Which is little script that takes a description of what I'm going to be doing for the next 25 mins and then logs that to a file with a time.
Finally I created this:
```
#!/bin/bash
x=`/usr/bin/osascript <<EOT
tell application "Finder"
    activate
    set myReply to text returned of (display dialog "Start 25 mins: Please put in task" default button 1 buttons {"OK"} default answer "" with icon 1)
end tell
EOT`
echo 'Text is: '$x
if [[ -z "$x" ]]; then
  echo "Done for the day"
else
  mkdir -p $HOME/work
  touch $HOME/work/daily_pomodoro.txt
  echo `date` $x >> $HOME/work/daily_pomodoro.txt
  delayed_mac 25 five_min
fi
``` 
Now all I have to do is run the script and I can keep track of what I'm doing all day.
