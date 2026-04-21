---
title: Sudoku Using Go Lang
date: 2019-05-01T09:03:54-04:00
draft: false
tags: [
    "algorithms",
    "golang",
]
---
## Algorithms
A friend of mine recently started looking for work and we were talking about the interview process.
She brought up the fact that often coding every day as a professional looks nothing like the toy problems in interviews.
This got me thinking about a surprise question that I had during an interview.
I was asked to create a Sudoku solver.
At the time I was pretty vexed.
I've never worked as a game programmer.
The position I was applying for was not a game programming position.
It was a standard full stack web development position.
Now I have a lot of thoughts on what interviews should look like and why.
One of them is that the interview should look at the relevant skills that are going to be used on a daily basis.
That is if you are going to be doing web development then the interview should be about well... web development.
I wish I could say this is a post about hiring and interviews, but it isn't.
Someday I'll come back and I'll write that post and probably gripe like an old man about hiring practices.
How those hiring practices can affect the diversity and capabilities of a team and all that jazz.
This post in a weird twist is actually about how I ended up finally writing that Sudoku solver.

## The code
[Here](https://github.com/charltonaustin/sudoku-solver) is the finished Sudoku solver.
There are some tests, but they are not super comprehensive.
Mostly this was a chance to do a toy problem and talk about it with my friend that is looking for a job.
I used the famed backtracking algorithm that is all over the internet.
In particular I used this description from [here](https://www.geeksforgeeks.org/sudoku-backtracking-7/)
```
  Find row, col of an unassigned cell
  If there is none, return true
  For digits from 1 to 9
    a) If there is no conflict for digit at row, col assign digit to row, col and recursively try fill in rest of grid
    b) If recursion successful, return true
    c) Else, remove digit and try another
  If all digits have been tried and nothing worked, return false
```
I modified it slightly since in building the solution finder I was also interested in generating valid completed boards.

## The dirty details
If we take a quick look at the algorithm you can see what I mean.
```go
func backTrace(b *board.Board) bool {
	for x := 0; x < 9; x++ {
		for y := 0; y < 9; y++ {
			value, err := b.Get(x, y)
			if err != nil {
				panic(err)
			}
			if value == 0 {
				for _, number := range randomNumbers() {
					err := b.Update(x, y, number)
					if err != nil {
						panic(err)
					}
					if b.IsValid() {
						if backTrace(b) {
							return true
						}
						b.Update(x, y, 0)
					} else {
						b.Update(x, y, 0)
					}

				}
				return false
			}
		}
	}
	return true
}
```

Notice that I have a function called randomNumbers.
It looks like this.
```go
func randomNumbers() []int {
	numbers := []int{1, 2, 3, 4, 5, 6, 7, 8, 9}
	rand.Shuffle(len(numbers), func(i, j int) { numbers[i], numbers[j] = numbers[j], numbers[i] })
	return numbers
}
```
I do that since when I was building the code I was actually interested in generating the board and so I needed to introduce some randomization in order to not get the same board repeatedly.
If we break down the description from above you can create a 1-1 mapping from the description to the code.

For instance, `Find row, col of an unassigned cell` directly maps to
```go
	for x := 0; x < 9; x++ {
		for y := 0; y < 9; y++ {
			...
		}
	}
```
While `If there is none, return true`, directly maps to the bottom return true.
An easier way to see it might be to just add comments like this.
```go
func backTrace(b *board.Board) bool {
	for x := 0; x < 9; x++ {
		for y := 0; y < 9; y++ { 
			// find empty square
			value, err := b.Get(x, y)
			if err != nil {
				panic(err)
			}
			if value == 0 { 
				for _, number := range randomNumbers() {
					// go through digits 1 to 9
					err := b.Update(x, y, number)
					if err != nil {
						panic(err)
					}
					// If there is no conflict for digit at row,
					if b.IsValid() {
					    // col assign digit to row, col and recursively try fill in rest of grid
						if backTrace(b) { 
						    //If recursion successful, return true
							return true 
						}
						//Else, remove digit and try another
						b.Update(x, y, 0) 
					} else {
						//Else, remove digit and try another (okay not exactly 1-1, but close)
						b.Update(x, y, 0) 
					}

				}
				// If all digits have been tried and nothing worked, return false
				return false 
			}
		}
	}
	//If there is none, return true
	return true 
}
```

## Why is this even here
For a few reasons.

1. One if you are asked a random Sudoku question and you have read this hopefully it helps you to get through the interview
1. I found writing this super relaxing and it started me on a completely different journey reading about algorithm design and such
    1. If you want to know more I suggest you read a few articles
    1. I read [this](https://www8.cs.umu.se/kurser/TDBA77/VT06/algorithms/BOOK/BOOK3/NODE124.HTM)
    1. I also read [this](https://algs4.cs.princeton.edu/home/)
    1. Both are fascinating and helped me to put some long standing questions about algorithms I had in context
1. Finally it also sparked a weird journey that led me [here](https://sites.math.washington.edu/~morrow/mcm/team2306.pdf)
    1. Sudoku and games are fun
    1. Solving the problems they present are interesting in their own right


## Next steps
Eventually I want to do something based on the paper about creating Sudoku boards.
Mostly I'm interested in modeling the problem as a set of rules or constraints with and option to brute force it so as to make all boards solvable.
That's a different future blog post though.
