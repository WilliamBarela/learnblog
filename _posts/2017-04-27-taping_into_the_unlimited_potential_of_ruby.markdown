---
layout: post
title:  "'tap'ing into the unlimited potential of Ruby"
date:   2017-04-27 08:30:26 +0000
---


For several years I have been a diehard Pythonist. Whenever friends and family asked me what the best language to start programming in is, I would invariably state that Python was great because it is dynamically typed, its syntax is one of the simplest and because there are so many good quality academic and scientific libraries available in it. Having seen the horrors of Perl from years before and having to schlepp through lists daily with `car` and `cdr` in Scheme, I envisioned a world in which there was not a more perfect programming language than Python. I had heard about Ruby, but read that it something of a 'reenvisioned Python'--certainly nothing worth my time to learn in the immediate.

> Initially, I was really perplexed by the `tap` method. 

When I very first started learning Ruby, my previous notion of the language was confirmed. Just slightly repackaged and sprinkled with another flavor of syntactic sugar, this was nothing more than a bloated Python with a heavy twist of object orientation...or so I thought.

It wasn't until I started to learn about the 'iterator' methods in Ruby that my opinion of the language started to be questioned. Python supports a similar class of functions, but it does not implement them as OOP methods. This is huge. Ruby reads like an eloquent agglutinative language like Turkish, while Python has to take in argument after argument, eating through your code like a snake. The sheer beauty of Ruby began to shake my time-tested belief that Python was king.

![](https://raw.githubusercontent.com/WilliamBarela/WilliamBarela.github.io/master/img/turkish-python-70.jpg)

But truly, where I was converted to a Rubyist was when I understood `tap`. Initially, I was really perplexed by the `tap` method. In the Ruby docs, this is the description:

>**`tap{|x| block } → obj` **

>Yields self to the block, and then returns self. The primary purpose of this method is to 'tap into' a method chain, in order to perform operations on intermediate results within the chain.

Reading this initially, I was confounded how any such method could ever be useful. A method which takes in itself and then returns itself? What kind of tautological hair brain idea was this? I confused. It was as convoluted as a snake eating its own tail. But then, one day, while watching Avi Flombaum from Flatiron giving a lecture on OOP, it clicked. I realized that at its most simplistic level, the parameter between the pipes in the block was a symbol for the datatype being tapped. This was amazing! This meant that you could self-contain code inside loop, for instance! No longer did I have to make a placeholder or iterator variable which sat outside of the loop to which it actually belonged! 

To illustrate this, imagine that your program somehow generates a `magic_number` variable. Now, let's say that you want to return an array of strings for the range of the magic number.  In python, you would be required to first initialize an array, and you would also have to initialize a dummy variable, i, which you would use as an iterator in a while loop (assuming you don't know how to use `for i in range(magic_number)`. After looping over and appending your array, you would then need to call the array to return its value. This is messy and it makes two variables which you don't need but for the process which you just completed. In Ruby, you can `tap` an empty array and then you can `tap` the magic number and loop shoveling in the results to the tapped array, and then it returns it without having your namespace clogged up with just another throw away variable. This can be seen below:

```
# Python using while loop:
magic_number = 10 

array = []
i = 0

while i < magic_number:
  i += 1
  array.append("num" + str(i + 100))

array
```

Versus:

```
# Ruby using while loop and tap.
magic_number = 10 

[].tap do |array|
  0.tap do |i|
    while i < magic_number
      i += 1
      array << "num #{i + 100}"
    end
  end
end
```

Although this is a rather contrived example and there are better ways to implement that code in both Ruby and in Python, it nevertheless illustrations lucidly one face: Ruby allows you to write tight, modular, beautiful code. You can keep your variables scoped to where they are relevant and only where they are relevant. That is the power of Ruby. That is why I am now a Rubyist for life.


