---
title: Pass by Reference, Pass by Value
published: true
description: A basic, Ruby-centric breakdown of what pass by reference and pass by value mean.
tags: #beginners #ruby #programming #computerscience
cover_image: https://thepracticaldev.s3.amazonaws.com/i/dlr54k81d4k615vo6ss8.jpg
---


The other day while I was grocery shopping, someone noticed my RubyConf t-shirt and asked, "So do you actually do anything in Ruby, or did you just go to the conference?" I said, "Well, right now I'm just an amateur, but I'm hoping to be good enough to get paid for it one day." Immediately he asked, "What's the difference between pass by reference and pass by value?"

I didn't know.

I mean, I could vaguely remember having heard those terms before, but there was no way I could articulate exactly what they meant. So I went home and set about learning it, and now I can gratefully say that I not only have an understanding of an aspect of programming languages that I didn't before, but I get to pass it along to others who are maybe in the same place I was.

But before we get there, it's important to understand a couple things first: computer memory, and variables.

***Memory***

![](https://thepracticaldev.s3.amazonaws.com/i/jy654rgdpwsii3ob5h2w.jpg)

Analogically, I like to think of memory as a bunch of tiny labeled boxes inside my computer. When I'm writing a program in Ruby, and I create an object, that object is located in a specific location in memory -- inside one of those labeled boxes. When I need that particular piece of data, it can be retrieved from that location in memory. *What is important to understand right now is that different objects will take up more or less memory depending on what they are. An integer takes up less memory space than a string.*

**Note:** This is a purposely oversimplified analogy. Memory, and the way Ruby utilizes it, is a much more complicated topic that I hope to cover in detail at a later date (read: once I feel like I understand it enough to explain it). But if at this point your mind is thinking about heaps and garbage collection, you should find something more advanced to read. If you don't know what those terms mean, don't stress about it right now and read on, my friend. You know what you need to know at this point.

***Variables and variable assignment***

When we talk about "value", we're talking about something that can be assigned to or stored in a variable, which is a descriptive phrase that points to the value assigned to it. In Ruby, a value can be a string, a boolean, an integer, a float, an array, a hash ... you get the idea. Assigning objects to variables allows us to use those objects in the methods we write. That's called "passing" the variable.

If I create a variable named

`best_sandwich_ever`

and (controversially) assign it the string object "Reuben",

`best_sandwich_ever = "Reuben"`

`Reuben` is the value I'm working with, and my program now understands that when I use the variable `best_sandwich_ever`, it needs to access the string object `Reuben` that is stored in my computer's physical memory. As soon as I assign my `Reuben` string to the variable, my operating system will store the string in a specific, numbered location in memory, so now when I use the `best_sandwich_ever` variable, I'm using my `Reuben` string.

```ruby
best_sandwich_ever = "Reuben"
puts "I love me a #{best_sandwich_ever}!"
"I love me a Reuben!"
```
If I want to create a method that returns the `best_sandwich_ever` variable, I will need to define the variable within that method, otherwise it will be outside the method's scope, and the method won't have access to it.

```ruby
best_sandwich_ever = "Reuben"

def fav_sammy
  return "I love me a #{best_sandwich_ever}!"
end

fav_sammy

# => NameError (undefined local variable or method `best_sandwich_ever' for main:Object)
```
However, I could *pass* the variable to the method as an argument.

```ruby
best_sandwich_ever = "Reuben"

def fav(sammy)
  return "I love me a #{sammy}!"
end

fav(best_sandwich_ever)
# => I love me a Reuben!
```
***Pass by reference***

So what exactly is happening here? When I pass `best_sandwich_ever` to the `#fav` method, I'm passing by reference, meaning that my variable is pointing to the exact location of my `Reuben` in memory. The variable is pointing to the number of the box that currently contains `Reuben`. Why? Because my string could change over time, and thus its memory requirements would change. What if I now want to specify that the best sandwich ever is a Reuben from my favorite pub?

```ruby
best_sandwich_ever << " (but only from The Old Spot)"

fav(best_sandwich_ever)
# => I love me a Reuben (but only from The Old Spot)!
```
That string now requires more memory than just the string "Reuben", so to save memory and processing time, Ruby modifies the string located at the original place in memory, rather than making a copy (which we'll get to as soon as we start talking about pass by value).

The takeaway here is that it is more memory efficient to modify data in its original location in memory, than to make a copy and modify the copy, unless the data we're referring to has low memory requirements, like integers. Accordingly, in Ruby, data types that have larger memory requirements (like strings and arrays) are passed by reference.

***Pass by value***

Similar to the my last example, if I create a variable named

`albert_einsteins_iq`

and assign it the integer object 160,

`albert_einsteins_iq = 160`

my computer's operating system immediately places that object at a specific, numbered location in memory. At this point, if I do something crazy like sum 160

```ruby
albert_einsteins_iq = 160
 # => 160
albert_einsteins_iq.digits.sum
 # => 7
```
it might seem like I lowered `albert_einsteins_iq` from 160 to 7 (ouch). However, that's not the case, because as soon as I called methods on `albert_einsteins_iq`, a copy of the data item `160` was made and assigned to a different location in memory than the original `160`, and when I summed the digits *the copy was altered, leaving the original alone.* We can prove this by calling the variable again and seeing what it returns.

```ruby
albert_einsteins_iq
# => 160
```
In Ruby, strings, arrays, and hashes are passed by reference.
Integers, floats, and booleans are passed by value.

**Note:** prior to Ruby 2.4, fixnum is also a data type that is passed by value, but since 2.4, fixnum and bignum became integer objects. Be aware also that pass by reference and pass by value will work a bit differently depending on which language we're talking about, but the basic concept is (mostly) the same.

So how would I have answered my grocery store friend if I had known this before? I would have said that pass by reference is when the actual memory address of the data item is passed, and when modified, the data item itself is modified. And pass by value is when the value of the data item is passed, and when modified, modifies the value leaving the data item at the original memory address unmodified.

If I pass an array containing my grocery list, what's being passed is the memory address of my array, and that array gets modified as I remove items from the list.

If I pass the number 1, what's being passed is the number 1, but at a separate memory address as the number 1 I originally assigned to a variable, and that is what is modified if I do arithmetic operations with it.

And then I would have asked him if he knew what aisle the LaCroix water was on.
