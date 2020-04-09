---
title: Problems With Cucumber And Chef
date: 2013-03-06T09:03:54-04:00
draft: false
---
So I've been working on getting some cucumber functional tests to run on a remote server lately and had tons of problems. The setup included using Electric Commander to run a gradle task, which in turn ran cucumber using jruby. It was lots of fun. Let me say.

This post is basically because cucumber put out a misleading "error" which when I searched google I couldn't get any help.  So for anyone else who finds themselves in some similar situation. Even if there is only one of you. I hope it helps. So this is what gradle was actually running.

```bash
jruby -S cucumber -v -t @api -f html -o/path/to/report/report.html
```

Which was giving me the output

```bash
Failed to load 'yml' programming language for file support/data/ap_atom_success.yml: 

no such file to load -- cucumber/yml_support/yml_language * support/.../*.yml [NOT SUPPORTED]

Failed to load 'json' programming language for file support/data/.../*.json: 

no such file to load -- cucumber/json_support/json_language  

* support/data/.../*.json [NOT SUPPORTED]
```

All of this of course was with an exit value of 1.

This of course left me scratching my head. I assumed that my tests weren't getting run. And that the failure was because I couldn't load 'yml' or 'json' programming language.  After a couple of searches on the internet I couldn't find anything useful.  So I left it for a bit and started asking around.  It turns out this has nothing to do with my failure. After looking in the report.html I saw that I there was actually a functional test failing and that despite the misleading cucumber output nothing was wrong with my cucumber run at all. I was getting the message because I was using -v. I'm not sure why -v would give that output.  According to cucumber --help verbose is supposed to show the files and features loaded. In any case. I switched gradle command to actually execute.

```bash
jruby -S cucumber -f pretty -t @api -f html -o/path/to/report/report.html
```

Which put out the failing test to the console (because I added -f pretty) as well as the report and got rid of the misleading error message (because I removed the -v).  Again I know this was just a silly problem, but that's why I made this post. I hope no one has to go through the pain I did ever again.