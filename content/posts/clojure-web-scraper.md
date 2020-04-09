---
title: Clojure Web Scraper
date: 2013-03-01T09:03:54-04:00
draft: false
---
Okay so another thing that I did this week was go to a clojure meet up. I really enjoyed it. There I met some really cool people like Anthony from WalmartLabs. In the end we created a hacker news scraper from scratch in a couple of hours. So here is a brief tutorial on how we did it. (Note I'm not that great with Clojure yet thought I do think it is rather awesome. So most of this is because Anthony is a rock star.)


#### Things you are going to need (or I expect to be installed):

1. Java 1.5 or greater
1. A text editor  
1. Lein

So first install lein which means going [here](https://raw.github.com/technomancy/leiningen/stable/bin/lein) and downloading the script.

Then move that script to somewhere in your path.

```bash
$ export  PATH=$PATH:/path/to/script
```

Next make a new lein project.

```bash
$ lein new hnharvest
```

Then cd into the project.

```bash
$ cd hnharvest
```

So we can add some text to project.clj.

```bash
$ vim   project.clj
```

Then make it look like this. If you don't know how to use vim use any text editor you like.


```clojure
(defproject hnharvest "0.1.0-SNAPSHOT"
  :description "FIXME: write description"
  :url "http://example.com/FIXME"
  :license {:name "Eclipse Public License"
            :url "http://www.eclipse.org/legal/epl-v10.html"}
  :dependencies [[org.clojure/clojure "1.4.0"]
                 [clj-http/clj-http "0.6.4"]
                 [enlive "1.1.1"]
                 [org.clojure/data.json "0.2.1"]])
```


Next we need to add our modify core.clj.

```bash
$ vim src/hnharvest/core.clj
```

Make it look like this.

```clojure
(ns hnharvest.core
  (:require [net.cgrand.enlive-html :as html]
            [clojure.string :as s]))

(defn fetch-url []
  (html/html-resource
   (java.net.URL. "http://news.ycombinator.com")))

(defonce content (fetch-url))

(defn parse-int [s]
  (try
    (Integer/parseInt s)
    (catch NumberFormatException e
      0)))

(defn hn-str->number [s]
  (-> (s/split s #"\s")
      first
      parse-int))

(defn get-info
  [{[{[points-string] :content}
     _
     _
     _
     {[comments-string] :content}] :content}]
  (when (and points-string comments-string)
   {:points (hn-str->number points-string)
    :comments (hn-str->number comments-string)}))

(defn go []
  (remove nil? (map get-info (html/select content [:td.subtext])))) 
```



Finally we need to open a repl session.

```bash
$ lein repl
```

Then we need to import our source files and change the name space and start the function.

```bash
user=> (use 'hnharvest.core)

user=> (ns hnharvest.core)

hnharvest.core=> (go)
```

This should spit out something like this:

```clojure
({:points 283, :comments 105} {:points 99, :comments 29} {:points 345, :comments 123} {:points 51, :comments 23} {:points 398, :comments 252} {:points 142, :comments 97} {:points 219, :comments 63} {:points 18, :comments 4} {:points 228, :comments 78} {:points 264, :comments 153} {:points 125, :comments 19} {:points 67, :comments 37} {:points 202, :comments 31} {:points 34, :comments 19} {:points 43, :comments 13} {:points 36, :comments 6} {:points 136, :comments 44} {:points 87, :comments 17} {:points 44, :comments 3} {:points 236, :comments 100} {:points 35, :comments 21} {:points 26, :comments 4} {:points 68, :comments 17} {:points 41, :comments 8} {:points 193, :comments 69} {:points 67, :comments 20} {:points 55, :comments 16} {:points 608, :comments 181} {:points 63, :comments 16})
```


This is just a current listing of points and comments from http://news.ycombinator.com/


I thought it was really cool how quickly we got something up and running with Clojure. I hope you did too. 