---
title: Why You Should Emulate Containers in Python
date: 
draft: true
description: "Why story points should be tied to time and what that does for you."
tags: [
    "development",
]
categories : [
    "management",
]
---
## The problem
Say that you have a list of items and each item has a price.
You would like to highlight the item or items that have the lowest price.
If all of the items are the same price you wouldn't like to highlight any of them.
We might have something like the following

```python
import sys


class Item:
  def __init__(self, name, price):
    self.name = name
    self.price = price


items = [Item("1", 1), Item("2", 2), ...]

lowest_price = sys.float_info.max
all_equal = True
last_price = items[0]

for item in items:
  if item.price != last_price:
    all_equal = False
  if item.price < lowest_price
    lowest_price = item.price
  item.last_price = item.price

printable_string = ""
if all_equal:
  for item in items:
    printable_string += item.name + str(item.price) + "\n"
else:
  for item in items:
    if item.price == lowest_price:
      printable_string += "**" + item.name + str(item.price()) + "**\n"
    else:
      printable_string += item.name + str(item.price()) + "\n"

print(printable_string.rstrip("\n"))
```

This is fine as long as our list is short.
Imagine though that our list was incredibly long.
You might realize that you have to traverse our items list twice.
The first time when we are finding the smallest price and testing if all prices are the same.
The second time when we want to print out the items.
Given a long enough list this would be less than ideal.

## The solution
One way to solve this problem would be to keep track of this data at list insertion time.
One way to do that is to emulate a container type.
It might look something like this.

```python
import sys

class Item:
  def __init__(self, name, price):
    self.name = name
    self.price = price


class ItemsIterator:
  def __init__(self, items):
    self._items = items
    self._index = 0

  def __iter__(self):
    return self

  def __next__(self):
    if self._index < len(self._items):
      return_value = self._items[self._index]
      self._index += 1
      return return_value
    raise StopIteration


class Items:

  def __init__(self):
    self._items = []
    self.lowest_price = sys.float_info.max
    self.all_equal = True
    self.last_price = None

  def add_item(self, item):
    current_price = item.price
    if self.lowest_price > current_price:
      self.lowest_price = current_price
    if self.last_price is not None and self.last_price != current_price:
      self.all_equal = False
    self.last_price = current_price
    self._items.append(item)

  def __len__(self):
    return len(self._items)

  def __getitem__(self, key):
    return self._items[key]

  def __setitem__(self, key, value):
    self._items[key] = value

  def __delitem__(self, key):
    del (self._items[key])

  def __iter__(self):
    return ItemsIterator(self)
```

Now we can rewrite our printing code to something like the following.

```python
items = Items()
items.add_item(Item("1", 1))
items.add_item(Item("2", 2))
...

printable_string = ""
if items.all_equal:
  for item in items:
    printable_string += item.name + str(item.price) + "\n"
else:
  for item in items:
    if item.price == items.lowest_price:
      printable_string += "**" + item.name + str(item.price()) + "**\n"
    else:
      printable_string += item.name + str(item.price()) + "\n"

print(printable_string.rstrip("\n"))
```

Now we only have to traverse the items container exactly one time.
Remember never optimize until you have a problem.
Also you you can read more details about emulating containers [here](https://docs.python.org/3/reference/datamodel.html#emulating-container-types).
You can read about emulating [generic](https://docs.python.org/3/reference/datamodel.html#emulating-generic-types), [callable](https://docs.python.org/3/reference/datamodel.html#emulating-callable-objects), and [numeric](https://docs.python.org/3/reference/datamodel.html#emulating-numeric-types) types at their respective links.
