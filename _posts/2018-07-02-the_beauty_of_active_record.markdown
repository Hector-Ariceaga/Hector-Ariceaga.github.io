---
layout: post
title:      "The Beauty of Active Record"
date:       2018-07-02 17:57:19 +0000
permalink:  the_beauty_of_active_record
---


Working with Active Record in Ruby is much like having a fully stocked and itemized toolbox in your garage. It makes the job easier. It's also faster, cleaner, and using the preexisting tools is intuitive. Inheriting from the Active Record class is like inheriting the tool box from a beloved family member. I am spared having to build the toolbox myself. I don't have to stock my own wrenches and screwdrivers. I can rely on the fine work of those who constructed this toolbox. 

Working with databases can be difficult. Don't make it harder on yourself by trying to build your own 'wrenches' and 'screwdrivers'. You will quickly find yourself violating the principle of DRY (Don't Repeat Yourself) when you realize that you are building 20 different versions of CRUD (Create, Read, Update, Delete) methods.  Let Active Record be your one-size-fits-most tool by abstracting a significant amount of your CRUD work.

### What can Active Record do for me?

In a nutshell, Active Record provides you with a dynamic and flexible ORM (Object-Relational Mapping) framework without you having to write one out yourself. It is capable of working with your plethora of classes while minimizing your code. It also helps reduce code smell caused by repetitious code. Rather drowning in lines of SQL, you can carry out complex operations with relative ease. Creating, retrieving, and manipulating objects are all painless with SQL-esque methods such as create_with, find, and order. 

### What's the catch?

If there is one at all, it would be that you will need to follow a few naming and schema conventions. However, these conventions actually benefit you. The conventions don't take long to learn and they will both standardize your code and spare you having to write much, if any, configuration code of our own. Best of all, in those times when you absolutely must break convention you have the ability to do so. 

### How do I start using Active Record?

Start by getting the Active Record library, either by running `$ gem install activerecord`​or including it in your Gemfile. If you do include the Active Record gem in your gemfile be sure to add `:require => 'active_record' `. Next, establish a connection to your database via `ActiveRecord::Base.establish_connection`. Finally, be sure to inherit from the appropriate Active Record class. `ActiveRecord::Base` will allow automated mapping between classes and tables, attributes and columns. `ActiveRecord::Migrate` will allow you to alter the schema of the database as it changes with time. 

### Where can I learn more?
Below are some links to additional documentation:

* [http://guides.rubyonrails.org/active_record_basics.html](http:// )
* [http://guides.rubyonrails.org/active_record_querying.html](http://)
* [http://edgeguides.rubyonrails.org/active_record_migrations.html](http://)





