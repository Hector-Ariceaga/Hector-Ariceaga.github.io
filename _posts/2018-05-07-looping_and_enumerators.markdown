---
layout: post
title:      "Looping and Enumerators"
date:       2018-05-07 15:50:56 -0400
permalink:  looping_and_enumerators
---

**I wanted to take the opportunity to quickly link to my personal blog (http://www.techector.com), which hosts more personal content such as learning goals, personal development, and interests. This blog will be serving as a more technical platform. **

### Please *yield* to oncoming *block* traffic!
Ruby has an exceptional way of further abstracting for loops using enumerator methods from the Enumerable class. These methods allow us to loop through arrays, hashes, or even arrays and hashes within one another, while also performing functions! These powerful methods come in a few different forms that behave slightly differently, but they all rely on two key components: *each* and *yield*.

The #each method will iterate through each element in an array (or key in a hash) much like a *for* loop will. The key difference is how the method utilizes the principle of yielding.

So what does it mean to *yield*? Simple, I'm glad you asked. It the passing of a value to a parameter, which can then be received by a block of code and operated upon.

Take the following for example:

```
class Dog
  
  def initialize(name) 
    @name = name
  end

  def dog
	#the line below allows us to pass the designated value to a block of code when called
    yield(@name) 
  end

end

rover = Dog.new("Rover")

#here we are invoking the method containing the *yield*
#the string between the pipes is arbitrary, in this case "name_of_dog", and is the receiver of our passed in value
#in this case @name
rover.dog do |name_of_dog|
#this line performs a function on the passed in value
  puts "This dog's name is #{name_of_dog}."
end

OUTPUT
This dog's name is Rover.

```

"But #yield is an object method. What does that have to do with arrays or hashes?" Great question! I can tell you are paying attention. The #each method is an array method, but can also be used on hashes, and remember I mentioned earlier that the #each method relies on the principle of yielding? Well, look at this:

```
array = [1,2,3,4,5]

array.each do |element|
  puts "The current element is #{element} and if I were to add 10 it would be #{element + 10}!"
end
 
 OUTPUT
The current element is 1 and if I were to add 10 it would be 11! 
The current element is 2 and if I were to add 10 it would be 12! 
The current element is 3 and if I were to add 10 it would be 13! 
The current element is 4 and if I were to add 10 it would be 14! 
The current element is 5 and if I were to add 10 it would be 15! 
=> [1, 2, 3, 4, 5] 
```

Notice that each element of the array is passed or yielded to the block when the array is called. The element is then operated on, in this case by the *puts* statement. Once the block is completed it will move to the next element. This continues until each element has been operated on. However, look what is returned by Ruby... It's the original array.

Let's refactor the code a bit. Let's say I just want to iterate through the array and add ten to each element and have the new values returned to me. Let's try this:

```
ARRAY = [1,2,3,4,5]

ARRAY.each do |element|
  element + 10
end

OUTPUT
=> [1, 2, 3, 4, 5] 
```

What happened? Why did I get back my original array? If you use pry you might notice that it is actually adding 10 to each element. Why then are we getting back the original array? Time to check the Ruby Documentation!

Credit to RubyDoc.org:

* each { |item| block } → ary click to toggle source
* each → Enumerator
* Calls the given block once for each element in self, passing that element as a parameter. **Returns the array itself.**

* If no block is given, an Enumerator is returned.

Look at the sentence in Bold (emphasis mine). We are asking Ruby to return the original array. That is why we are getting our original array, even though we are making changes to it. So what do we do? This where our Enumerable class methods come in. Remember, there are several. So to find one that works for our original goal, we will need to read through the Ruby Documentation. For the sake of simplicity, let’s try using the #collect enumerator. This is how the documentation describes the method.

* collect { |obj| block } → array click to toggle source
* collect → an_enumerator
* **Returns a new array with the results of running block once for every element in enum.**

* If no block is given, an enumerator is returned instead.

Look at the bolded section (emphasis mine). This might work. Let's give it a try.

```
ARRAY = [1,2,3,4,5]

ARRAY.collect do |element|
  element + 10
end

OUTPUT
=> [11, 12, 13, 14, 15] 
```

Would you look at that! We did it! 

These enumerators extend the functionality of traditional *for* loops by allowing us to *yield* values to a block of code and determine what type of data we want returned (even boolean values!). They also allow us to go deeper into our hashes or arrays, which may contain yet more hashes and arrays, without constantly writing more *for* loops. Looping correctly and efficiently is an essential skill and in the vain of KISS (Keep It Simple Stupid) and DRY (Don't Repeat Yourself) it is important to equip yourself with skills that allow you to do so. Enumerators from Ruby's Enumerable class are just some of those tools. Hopefully, with this brief introduction into their use, you will be more inclined to look into how these tools can help you.



