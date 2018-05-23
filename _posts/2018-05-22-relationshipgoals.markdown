---
layout: post
title:      "#RelationshipGoals"
date:       2018-05-23 03:27:50 +0000
permalink:  relationshipgoals
---


### OOP and You
For those of you even slightly familiar with object orientated programming (OOP) it comes as no surprise that one of the primary principles of OOP is to create code that mirrors real life. By mirroring how objects exist in real life, OOP provides some great benefits to the programmer including code reusability, code flexibility, and an effective system of problem solving. In a way, it makes intuitive sense that handling code that resembles the reality we deal with on a day-to-day basis is easier than working with something more abstract. 

With a model focused on objects, we can begin to design code with expectations on how things should exist and behave based on our own life experiences. For example, take an object in the real word, such as this blog post. It has properties, such as length and content. This post also has states, such as published or unpublished. It may even have behaviors, like linking to other webpages. In short, the object, this blog post, by virtue of existing, has traits and maybe even behaviors. With OOP, we have objects in our code that operate in the same way. 

### Where it gets tricky
Imagine a universe where objects, such as the device your reading this on right now, could only interact with themselves. They would have traits and behaviors, but none of the behaviors would be in relationship to other objects. For example, you could determine how many items a grocery basket holds, but you wouldn't be able to determine the cost of each item. The cost of the item is a trait of the item itself and the grocery basket doesn't have a relationship to the items, and so doesn't have access to the items (or their traits) in this crazy upside world. Each object would exist in complete isolation. Imagine phones with no way to interact with other phones, cars that couldn't take in or burn fuel, people that couldn't talk to other people! That's a pretty boring and static universe. Obviously, in reality, objects interact with one another all the time. Well guess what? This is true in OOP, as well. 

Often, we want objects in our code to be able to interact with other objects in our code. This is done by establishing relationships between objects. Again, in OOP, we want objects in our code to mirror real life, and in reality, objects have relationships to other objects. Where it gets tricky is remembering that relationships may be reciprocal. In other words, objects in a relationship might be bound equally. Each object should "know" what their relationship to the other object is. For example, let's say I own a pen. The following two statements are true: "I own the pen" AND "The pen belongs to me." While the statements seem to be saying the same thing, they aren't. For example, if you were to ask me what I own, I would show you the pen. But what if you only had the pen and you wanted to know who it belonged to? Well, you could ask the pen, but that seems silly right? How would the pen know it's relationship to me? I know that I own it, but the pen doesn't know that it belongs to me. What if I put my name on the pen? Suddenly, the relationship is reciprocated. If you see the pen, you can deduce it belongs to me. Get it? Both objects must "know" their relationship to one another for it to be reciprocal. Luckily, we can add relationships between the objects in our code in much the same way. We can set up the relationship in our code such that you could ask me what I own, and I would show you the pen and if you were to ask the pen, "Who do you belong to?" and it would say me. 

### How the magic happens
It can get confusing quickly if we don't stop and really try to think about the relationship we are modeling. Let's try modeling the relationship between me and some pens, like we discussed above.

First, we need to set up an Owner class and Pen class. These will serve as the blueprint for individual owners and pens.

```
class Owner
end
```

```
class Pen
end
```

The next step will be to determine what traits we want our owners and pens to have and whether we want these to be changeable by our object. For this example, let's say that owners have names and pens have brands. In addition, I want owners to be able to change their names and what pens they own. Also, I want pens to be able to change their owners, but not their brands. Those are some real-life restrictions. Now, let's model them in our code.

```
class Owner
 attr_accessor :name, :pens #this allows us to set/get names of owners and set/get the pens they have
end
```

```
class Pen
  attr_accessor :owner #this allows us to set/get the owner of a pen
  attr_reader :brand #this allows us to get the brand of a pen, but not alter it
end
```

You will notice that at this point the Owner class is set up to have our owners possess a :pens trait and the Pen class is set up to have our pens have an owner, but as of right now, the classes, and in turn our objects, aren't communicating. Before we establish the relationship between our objects let's set up our classes further, so we can make some actual objects.

```
class Owner
 attr_accessor :name, :pens 
 
 def initialize(name)
    @name = name #when an owner object is created, it gets its name set to the name argument passed in by the user
    @pens = [] # sets the currently owned list of pens to an empty array
  end
end
```

```
class Pen
  attr_accessor :owner 
  attr_reader :brand 
	
	def initialize(brand, owner)
    @brand = brand #when a pen object is created, it gets a brand set to the argument passed in by the user
    @owner = owner #when a pen is created it has its owner set to the argument passed in by the user
  end
end
```

Okay, so now we can make owners with names that have pens and the ability to make pens that have brands and owners. What are we missing? We are still missing the relationship! This next piece requires us to work through a little more logic. What do we need to do? Well, adding pens to our owner's list of owned pens is a good place to start. Let's do that.

```
class Owner
  attr_accessor :name, :pens
  
  def initialize(name)
    @name = name
    @pens = []
  end
  
  def add_pen(pen)
    pens << pen #adds a pen argument passed in by a user into the owner object's collection of pens
	end
end
```

So now we can add pens to our owner object. This is great! We can ask an owner for his pens. Are we done? Nope. Remember, reciprocal relationships! We need to let our pen know that it has a new owner. This will require us to refactor our code in both classes. Let's do it!

```
class Owner
  attr_accessor :name, :pens
  
  def initialize(name)
    @name = name
    @pens = []
  end
  
  def add_pen(pen)
    pens << pen
    pen.owner = self #set the owner of the passed in pen to this owner object
  end
end
```

```
class Pen
attr_accessor :owner
attr_reader :brand
  
  def initialize(brand, owner)
    @brand = brand
    owner= (owner) #calls the method below
  end
  
  def owner=(owner) #custom setter
	  @owner= owner 
    owner.add_pen(self) #add this pen object to the list of pens that belong to owner
  end
end
```

Now we have a new problem. We have an infinite loop where our Pen and Owner classes never stop referring to one another. They are communicating too much! How is that for a real-world problem?! We need to refactor our code once more so that that our methods only communicate if they haven't communicated already, thus breaking the loop.

```
class Owner
  attr_accessor :name, :pens
  
  def initialize(name)
    @name = name
    @pens = []
  end
  
  def add_pen(pen)
    pens << pen
    pen.owner = self unless pen.owner == self #if this object is already the owner of this pen, we don't want to set it again
  end
end
```

```
class Pen
attr_accessor :owner
attr_reader :brand
  
  def initialize(brand)
    @brand = brand
  end
  
  def owner=(owner)
    @owner = owner
		owner.add_pen(self) unless owner.pens.include?(self) #reciprocates the check, if this pen already belongs to the owner, we don't want to add it
  end
end
```

That's it! Now we have set up two objects that can relate to one another. We can even ask them about their relationship to one another. Let's play around.

```
me = Owner.new('Hector') #make an owner
nice_pen = Pen.new('bic') #make a pen

me.add_pen(nice_pen) #inform objects of relationships to one another

#now we can ask for information based on their relationships

me.pens 
#=>a list of pens owned by me, will include our nice_pen object

me.pens.first.brand 
#=> brand of first pen owned by me ('bic')

nice_pen.owner 
#=> owner of nice_pen object (me)
```

Cool right? This is the power of modeling relationships in our code similar to how relationships exist in reality. More objects can be added, and the relationships will be maintained. If you are interested, you can even go a step further by exploring how objects can relate to one another via 'has many through' relationships. That is outside the scope of this post, but you can read more here [http://guides.rubyonrails.org/association_basics.html].


