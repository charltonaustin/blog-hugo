---
title: Graphing Change Vs Cyclomatic Complexity
date: 2019-06-10T09:03:54-04:00
draft: false
---
### The problem

You want to know where you should be refactoring, and you are not sure where to start.
Or, you would like to better understand why you have more bugs in one part of the code than another.

## One approach
One thing that I like to look at when this happens is cyclomatic complexity.
That often isn't enough to get the full picture of what is happening with code.
Another place I like to look is how often a file is changing.

## Some theory
There are many good tools out in most languages where you can measure the cyclomatic complexity of your source code.
At the Tuesday Company with our python code we use [radon](https://pypi.org/project/radon/).
For our JS code we use [plato](https://github.com/es-analysis/plato)
These are just two options in an ocean of different static analysis tools that you can find with a quick Google search.
I like to use cyclomatic complexity, but there are benefits and drawbacks to using it as a tool for measuring complexity. 
I would suggest reading articles like this [one](https://www.cqse.eu/en/blog/mccabe-cyclomatic-complexity/).
Or articles like this [one](https://www.softwareyoga.com/cyclomatic-complexity/).
They should help give you some background understanding if you don't already have it.
I like it because while it is an imperfect measure, I find it a good starting place for conversations about coding quality.
I believe though you could do this exercise with any number of measurements in a variety of ways.
You could find some way to condense multiple measurements to a single value like the [maintainability index](https://radon.readthedocs.io/en/latest/intro.html#maintainability-index) or you could simply have a multidimensional graph.
I personal like to keep things visual and simple so I use cyclomatic complexity with all of it's flaws as a single measure.
I then take that measure and graph it how often code changes.
To measure that, I again use an imperfect approximation.
In particular since I use git for version control so I use the number of commits to a file as a rough approximation.


## The process for git
To get a useful measure from git I general use a command like this one.
`git log --name-only --pretty=format: | grep <folder name> | sort | uniq -c | sort -nr > changes.txt`
This produces an output with something like the following:
```
100 <folder name>/path/to/file/a.py
40 <folder name>/path/to/file/b.py
10 <folder name>/path/to/file/c.py
2 <folder name>/path/to/file/d.py
1 <folder name>/path/to/file/e.py
...
```
The first column is the number of times a commit has been made to that file and the second column is the file name.

## The process for python
For python I would do something like this.
`radon cc -s -j ./* | jq 'with_entries(.value |= .[].complexity)' | jq 'to_entries' | jq '.[]' | jq '"\(.value) \(.key)"' |  tr -d '"' | sort -rn > cc.txt`
This produces a file with something like the following:
```
26 <folder name>/path/to/file/a.py
24 <folder name>/path/to/file/b.py
20 <folder name>/path/to/file/c.py
19 <folder name>/path/to/file/d.py
```
Here the first column is the cyclomatic complexity and the second column is the file name.

## The process for JS
For JavaScript I like the plato tool which gives me a directory with a bunch of information.
I simply process the JSON that it produces and do something very similar.
`rm -rf report && plato -r -d report app && cat report/files/**/report.json | jq '"\(.complexity.methodAggregate.cyclomatic) \(.info.file)"' | tr -d '"' | sort -rn > cc.txt`
This produces a file with something like the previous output just JS instead of PY.

## Graphing all of this
Finally I take the out puts and I create graphs for them with the following script.
```python
import os
import sys

import matplotlib.pyplot as plt
import mplcursors

base_dir = os.environ.get('PROJECT_HOME')
if not base_dir:
  print("Please set PROJECT_HOME environment variable")

project_name = sys.argv[1]
changes_file_name = f"{base_dir}/{project_name}/changes.txt"
cc_file_name = f"{base_dir}/{project_name}/cc.txt"


def get_data(file_name_):
  data = {}
  names = set()
  with open(file_name_, 'r') as file:
    for cnt, line in enumerate(file):
      try:
        d, name = line.strip().split(" ")
        data[name] = d
        names.add(name)
      except ValueError:
        print(f"skipping line\n {line}")
  return data, names


ccs, ccs_keys = get_data(cc_file_name)
changes, change_keys = get_data(changes_file_name)
file_names = change_keys.intersection(ccs_keys)
x = []
y = []
labels = []
fig, ax = plt.subplots()
for file_name in file_names:
  x.append(float(ccs[file_name]))
  y.append(float(changes[file_name]))
  labels.append(file_name)

lines = plt.plot(x, y, 'bo')
cursor = mplcursors.cursor(hover=True)
cursor.connect("add", lambda sel: sel.annotation.set_text(labels[sel.target.index]))
plt.xlabel("complexity")
plt.ylabel("number of times changed")
plt.title(f"complexity vs churn {project_name}")
plt.show()
```
Now, none of this graphing code really depends on the project.
All you really need are two files that are named cc.txt and changes.txt.
It reads those and then creates a graph where the x label comes from cc.txt and the y label comes from changes.txt.
It only really depends on [matplotlib](https://matplotlib.org/) and [mplcursors](https://mplcursors.readthedocs.io/en/stable/)
You could easily do this in any language that you prefer.

## What does it all mean
This graph can quickly and fairly easily track down possible trouble spots in your code.
The graph that is produced breaks into 4 quadrants.
The lower left hand has files that have low cyclomatic complexity and that rarely change these guys are the good files.
The lower right side has files that have high cyclomatic complexity and they rarely change these. These may be fixed by reducing complexity, but aren't high priority.
The upper left side has low cyclomatic complexity and the files change frequently these can maybe be fixed by breaking them into smaller pieces, but aren't high priority.
The upper right side has files that have high cyclomatic complexity and the files change frequently.  These guys are my red flags.
I tend to see if I can break them up so that they change less and/or I look for ways to reduce the complexity.

## References
I definitely don't want to give the wrong impression.
I read about this on the internet and decided to give it a go some time ago.
Since then I have used it in every project that I have been on to understand the code better.
If you would like to see where I got these ideas please read [here]( https://www.stickyminds.com/article/getting-empirical-about-refactoring)
There was also another article that helped me a great deal, but I couldn't find it for this blog post.
So if you are out there please let me know so I can show my references.
