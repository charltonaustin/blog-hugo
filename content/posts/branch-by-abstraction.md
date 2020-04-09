---
title: Branch By Abstraction
date: 2018-05-07T09:03:54-04:00
draft: false
---
## The problem
I want to write code that is consistently shared with the rest of the team, but I also want trunk based development.

## What is branch by abstraction
[Paul Hammant](https://www.linkedin.com/in/paulhammant/) a friend and mentor of mine,
has two great articles explaining branch by abstraction [here](https://trunkbaseddevelopment.com/branch-by-abstraction/) and [here](https://paulhammant.com/blog/branch_by_abstraction.html).
The basic concept is pretty easy.
Instead of having long lived branches in your version control system, you simply create an abstraction that allows you to change the behavior with only a few minor changes.
As great as this concept is, there is one thing missing.  During any explanation, I find it difficult to give decent real world examples of branch by abstraction.
Usually there are a lot of diagrams and talk about concepts, but very few solid examples.
My team has been struggling with doing this in a coherent way, so here are some examples of what it might look like in the wild.


## Example with a function
They are all going to be based on [this](https://martinfowler.com/bliki/BranchByAbstraction.html) article from Martin Fowler.
Originally you might have:
```python
class MarioCar(object):

  @staticmethod
  def drive():
    print("Let's-a-go!")


def mario_kart():
  MarioCar().drive()


def mario_kart_arcade():
  MarioCar().drive()


def mario_kart_ds():
  MarioCar().drive()


mario_kart()
mario_kart_arcade()
mario_kart_ds()
```

Then later you might have:
```python
class MarioCar(object):

  @staticmethod
  def drive():
    print("Let's-a-go!")


def get_car(system_name):
  return MarioCar()


def mario_kart():
  get_car('mario_kart').drive()


def mario_kart_arcade():
  MarioCar().drive()


def mario_kart_ds():
  MarioCar().drive()


mario_kart()
mario_kart_arcade()
mario_kart_ds()
```

Eventually you get here:
```python
class MarioCar(object):

  @staticmethod
  def drive():
    print("Let's-a-go!")


def get_car(system_name):
  return MarioCar()


def mario_kart():
  get_car('mario_kart').drive()


def mario_kart_arcade():
  get_car('mario_kart_arcade').drive()


def mario_kart_ds():
  get_car('mario_kart_ds').drive()


mario_kart()
mario_kart_arcade()
mario_kart_ds()
```

Now you want to deal with some of the clients:
```python
class MarioCar(object):

  @staticmethod
  def drive():
    print("Let's-a-go!")


class WarioCar(object):
  @staticmethod
  def drive():
    print("Here I go!")


def get_car(system_name):
  ready_systems = ['mario_kart']
  if system_name in ready_systems:
    return WarioCar()
  return MarioCar()


def mario_kart():
  get_car('mario_kart').drive()


def mario_kart_arcade():
  get_car('mario_kart_arcade').drive()


def mario_kart_ds():
  get_car('mario_kart_ds').drive()


mario_kart()
mario_kart_arcade()
mario_kart_ds()
```

Finally all the clients are over:
```python
class MarioCar(object):

  @staticmethod
  def drive():
    print("Let's-a-go!")


class WarioCar(object):
  @staticmethod
  def drive():
    print("Here I go!")


def get_car(system_name):
  return WarioCar()


def mario_kart():
  get_car('mario_kart').drive()


def mario_kart_arcade():
  get_car('mario_kart_arcade').drive()


def mario_kart_ds():
  get_car('mario_kart_ds').drive()


mario_kart()
mario_kart_arcade()
mario_kart_ds()
```

So we can delete the last implementation
```python
class WarioCar(object):
  @staticmethod
  def drive():
    print("Here I go!")


def get_car(system_name):
  return WarioCar()


def mario_kart():
  get_car('mario_kart').drive()


def mario_kart_arcade():
  get_car('mario_kart_arcade').drive()


def mario_kart_ds():
  get_car('mario_kart_ds').drive()


mario_kart()
mario_kart_arcade()
mario_kart_ds()
```

## Example with a feature flags
Again originally you might have:
```python
class MarioCar(object):

  @staticmethod
  def drive():
    print("Let's-a-go!")


def mario_kart():
  MarioCar().drive()


def mario_kart_arcade():
  MarioCar().drive()


def mario_kart_ds():
  MarioCar().drive()


mario_kart()
mario_kart_arcade()
mario_kart_ds()
```

Then you move to this guy
```python
use_wario_saying = False


class MarioCar(object):

  @staticmethod
  def drive():
    if use_wario_saying:
      print('Here I go!')
    else:
      print("Let's-a-go!")


def mario_kart():
  MarioCar().drive()


def mario_kart_arcade():
  MarioCar().drive()


def mario_kart_ds():
  MarioCar().drive()


mario_kart()
mario_kart_arcade()
mario_kart_ds()
```

Do some testing make sure everything is the way you want before you get here:
```python
use_wario_saying = True


class MarioCar(object):

  @staticmethod
  def drive():
    if use_wario_saying:
      print('Here I go!')
    else:
      print("Let's-a-go!")


def mario_kart():
  MarioCar().drive()


def mario_kart_arcade():
  MarioCar().drive()


def mario_kart_ds():
  MarioCar().drive()


mario_kart()
mario_kart_arcade()
mario_kart_ds()
```

Finally you are here:
```python
class MarioCar(object):

  @staticmethod
  def drive():
    print('Here I go!')


def mario_kart():
  MarioCar().drive()


def mario_kart_arcade():
  MarioCar().drive()


def mario_kart_ds():
  MarioCar().drive()


mario_kart()
mario_kart_arcade()
mario_kart_ds()
```

## Commenting functions
Here's another example that I think is under utilized.
Originally
```python
def mario_drive():
  print("Let's-a-go!")


def mario_kart():
  mario_drive()


def mario_kart_arcade():
  mario_drive()


def mario_kart_ds():
  mario_drive()


mario_kart()
mario_kart_arcade()
mario_kart_ds()
```

But the end of the day comes and you are out of time so you just comment out.
```python
def mario_drive():
  print("Let's-a-go!")


def wario_drive():
  print('Here I go!')


def mario_kart():
  # wario_drive()
  mario_drive()


def mario_kart_arcade():
  mario_drive()


def mario_kart_ds():
  mario_drive()


mario_kart()
mario_kart_arcade()
mario_kart_ds()
```
Day two comes along and you've testes for mario kart the original, but haven't finished testing the arcade version.

```python
def mario_drive():
  print("Let's-a-go!")


def wario_drive():
  print('Here I go!')


def mario_kart():
  wario_drive()


def mario_kart_arcade():
  # wario_drive()
  mario_drive()


def mario_kart_ds():
  mario_drive()


mario_kart()
mario_kart_arcade()
mario_kart_ds()
```


Before you know it everything is working.

```python
def mario_drive():
  print("Let's-a-go!")


def wario_drive():
  print('Here I go!')


def mario_kart():
  wario_drive()


def mario_kart_arcade():
  wario_drive()


def mario_kart_ds():
  wario_drive()


mario_kart()
mario_kart_arcade()
mario_kart_ds()
```

Don't forget to remove unused code.
```python
def wario_drive():
  print('Here I go!')


def mario_kart():
  wario_drive()


def mario_kart_arcade():
  wario_drive()


def mario_kart_ds():
  wario_drive()


mario_kart()
mario_kart_arcade()
mario_kart_ds()
```

## Some final thoughts
You might use inheritance when you have a typed language, or polymorphic dispatch when you are using a functional language.
The trick is to figure out an abstraction where you have a single choke point.
That way at the end of the day it is easy to clean up and turn on and off.
