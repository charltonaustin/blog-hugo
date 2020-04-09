---
title: Top Down Vs Bottom Up Api Design In Clojure Part One
date: 2016-04-26T09:03:54-04:00
draft: false
---
### Recently in my last post...

Recently I posted about the difference between [top down vs bottom up](http://www.charltonaustin.com/2016/04/top-down-vs-bottom-up-api-design-in.html) design in Clojure, if you haven't read it then you should. This post is the second part about the differences in both code quality and experience between tope down and bottom up approaches.  You can see the commits [here](https://github.com/charltonaustin/inside_out_ouside_in/commits).


### First impressions

It took me 14 functions using the outside-in method vs 15 functions with the inside-out method. But the number of functions doesn't really matter. What's more interesting maybe is the number of characters. It was 2043 for inside-out and 2122 outside-in (without white space). They were pretty close; there wasn't much of a difference; and counting characters isn't all that important (though my usual philosophy is 'less is better').  What about readability? Well my opinion is outside-in is marginally better. The main loop is less convoluted. Perhaps because I don't do any stupid exit business.


For the inside-out I had:

```clojure
(defn -main
  [& args]
  (loop [a-board (board)
         number-of-moves 0]
    (if (>= number-of-moves 9)
      (do (print-to-screen (str-board a-board))
        (exit-now!))
      (do (print-to-screen (str-board a-board))
        (let [player-function (nth turn-function number-of-moves)
              new-board (player-function a-board)
              maybe-winner (check-for-winner a-board)]
          (if maybe-winner
            (do              (println (str maybe-winner " has won!"))
              (recur new-board 9))
            (recur new-board
                   (inc number-of-moves))))))))

```

Outside-in:

```clojure
(defn -main
  [& args]
  (loop [board (vec (range 1 10))
         number-of-moves-made 0
         get-next-move-fn (get-player-move-fn number-of-moves-made)]
    (print-to-screen (string-board board))
    (if (= number-of-moves-made 9)
      nil
      (do (let [player-move (get-next-move-fn board)
              new-board (update-board board player-move)
              number-of-moves-made (inc number-of-moves-made)
              winner? (check-for-winner new-board)]
          (if winner?
            (do (print-to-screen (string-board new-board))
                (println (str "Player " (:character player-move) " won!")))
            (recur new-board number-of-moves-made (get-player-move-fn number-of-moves-made))))))))
```

Obviously only an idiot would write the exit function in the first one. I didn't notice this fact until after I had written the second one. I was focusing on writing the outermost layer first and the way I had written it didn't influence the design of the main loop.


The tests were significantly different. With the outside-in method I wrote most of the human tests in the (-main) test code. Looking back I would definitely refactor that to use a stub implementation in the main test. Other than that there seems to be less test code in general, but I feel pretty confident about both. I added in the same src and test code for finding the winners since I had removed it from my todo list.

### Things I learned

First towards the end of the exercise I was often using the main loop to write the function interface and then stubbing out the implementation using with-redefs for underlying functions. I actually like this, but because this was such a simple example I'm not sure how complex it would be to mock other functions. I liked it however because by the end I think this started isolating my tests more and stopped me from testing everything in the (-main) test. While there isn't much of a difference in the outcome, I did like the outside-in method more.
