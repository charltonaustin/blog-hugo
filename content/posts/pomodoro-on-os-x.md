---
title: Pomodoro On Os X
date: 2016-11-03T09:03:54-04:00
draft: false
---
### Pomodoro
If you haven't ever used the [pomodoro technique](http://pomodorotechnique.com/) then you are missing out.
It is a great way to stay focused on one task at a time.
It is also a good way to get yourself moving regularly.
There is just one thing I don't like about it.
In order to have a good timer you usually have to have something that bothers your neighbor at work.
For a while I was using my phone timer which worked okay, but I'm pretty sure my work mates hated me.
### Solution
To solve this problem I used a *nix utility called at and an OSX utility called osascript.
If you haven't used it the [at command](http://www.computerhope.com/unix/uat.htm) schedules a command to execute sometime in the future.
The osascript is a hook for shell scripts into AppleScripts.
Putting those two things together I came up with this.

```
at now + 25 minutes <<< "osascript -e 'tell app \"System Events\" to display dialog \"Take A Break Pomodoro :)\"'"
```

This is a command that you can put into a script and run to set up a pomodoro timer that will pop up 25 minutes later.

### Next steps
The next steps would be having a hook so then when you hit okay it starts up a 5 min timer that has another pop up.
That next pop up should set another timer for 25 minutes and then maybe I will never forget to set up my next pomodoro timer.
