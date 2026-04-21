---
title: Unit Testing With Higher Order Functions
date: 2016-04-13T09:03:54-04:00
draft: false
tags: [
    "python",
    "testing",
    "functional",
    "development",
]

---
Recently I was writing an interesting bit of Python software that had a couple of layers. There were some high level objects that dealt with some domain logic and then some lower level objects that did more of the nitty gritty interactions with the operating system. When I first started thinking about writing the lower level interface, I was a little afraid that writing the unit tests was going to be difficult. Wrapping up Sys.out or other IO operations in Java is a standard practice. In the past I have had some trouble patching Python modules.


We start out with a low level function that does some IO operations.

```python
def process(line):
    line.replace("a", "b")

def do_some_work_on_a_file(input_file, output_file):
    output = open(output_file, "w")
    with open(input_file) as input_:
        for line in input_:
            output.write(process(line))

```

Our test looks something like this.

```python
import tempfile

def test_do_some_work(self):
    input_file = tempfile.NamedTemporaryFile()
    output_file = tempfile.NamedTemporaryFile()

    with open(input_file.name) as file_io:
        file_io.write("a")

    do_some_work_on_a_file(input_file.name, output_file.name)

    with open(output_file.name) as file_:
        self.assertEquals(file_.readlines(), "b")

```

It would be better to refactor this because as it stands the unit tests will take a while because of Python's open function.


Fortunately we can do something about this. We simply pull the low level OS function as a parameter with the default set as Python's open function. Then we can use an in, memory alternative to a file in this case StringIO for the testing. Here's what the refactored code looks like:

```python
def do_some_work_on_a_file(input_file_name,
                           output_file_name,
                           open_=open):
    output = open_(output_file_name, "w")
    with open_(input_file_name) as input_:
        for line in input_:
            output.write(process(line))
```

And the refactored test code looks like:

```python
def fake_open_creator():
    callers = []

    def fake_open(name, mode=None, buffering=None):
        io = StringIO()
        if mode is None:
            io.write("a")
        callers.append((name, io))
        return io

    return callers, fake_open

def test_do_some_work(self):
    callers, fake_open = fake_open_creator()

    do_some_work_on_a_file("input_file_name",
                           "output_file_name",
                           fake_open)

    self.assertEquals(callers[1][1].getvalue(), "b")
```

Now there are a couple of problems with this code. The StringIO object doesn't have an __enter__, __exit__ method so it will fail on the with statement, but you can monkey patch or use a proper mocking framework to fix this.

### So what did this get us?

There are a couple of really nice things we have done here. First, we don't have to worry about global state. The tests have no way of interacting with one another which can happen when you are patching modules and functions. Second, the function definitions is very close to the invocation. You could create a class that wires in the dependency, but I find it is often simpler and leaves the wiring closer to the invocation. Finally, this is a fairly flexible and cheap way of mocking out a function. You can get the same results by using a mocking framework, but sometimes that can obscure the intention of the test.
