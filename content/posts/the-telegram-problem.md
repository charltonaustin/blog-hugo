---
title: The Telegram Problem
date: 2019-11-15T09:03:54-04:00
draft: false
tags: [
    "algorithms",
    "toy problems",
    "javascript",
]
---
### The problem

Write a program that takes a number w, then accepts lines of text and outputs lines of text, where the output lines have as many words as possible but are never longer than w characters.
Words may not be split, but you may assume that no single word is too long for a line. 
[Source](http://c2.com/cgi/wiki?TelegramProblem)

### Solution

This is a straightforward problem when it comes down to it, but it takes some thinking to get there.
We need to work through the different edge cases to figure out exactly how to make this work.
I would like to create a solution that deals with channels allowing users to send delete characters and such.
I think it would be an interesting problem in itself.
That being said, my solution starts out by initializing the output and current output so that we don't have special cases in our for-loop.
It also sanitizes and formats the information that we will be walking over.
Next it loops through and checks to see if the line will grow to long.
If it won't, we add to the current line output; if it will, we save the current line output to the total output and then initialize the next line output.
Finally, on our way out, we flush the last partial line.

```javascript
var createWriter = function(w){
  var writer = function(lines){

    var output = "";
    var words = lines.replace("\n", " ").split(" ")
    var currentOutput = words[0];
    var currentLineLength = words[0].length;
    
    for(var i = 1; i< words.length; i++){
      
      var word = words[i];
      
      if(currentLineLength + word.length + 1 > w){
        output += currentOutput + "\n";
        currentOutput = word;
        currentLineLength = word.length;
      } else {
        currentOutput += " " + word;
        currentLineLength += word.length + 1;
      }
    
    }
    
    output+= currentOutput;
    
    return output;
  }
  
  return writer;
}
```

If you'd like to see the full code please see my daily [toy problem](https://github.com/charltonaustin/toy-problems/tree/master/telegram-problem) exercises that I've been working on.
It includes tests and a README.
