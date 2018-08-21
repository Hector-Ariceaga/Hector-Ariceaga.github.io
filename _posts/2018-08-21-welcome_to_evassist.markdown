---
layout: post
title:      "Welcome to EvAssist"
date:       2018-08-21 20:33:17 +0000
permalink:  welcome_to_evassist
---


EvAssist is a simple web application that I designed primarily using Sinatra and Active Record. It utilizes SQLite3 for a database and bcrypt to hash passwords. The Twitter Bootstrap was used as the front-end framework. The purpose of the application is to allow users, specifically photographers, to register a secure account which permits them to create, view, and edit events and tasks in a list format (similar to to-do lists or checklists) to track their work, be it upcoming or in the past.

The application is comprised of three models: Users, Events, and Tasks. Users are able to navigate through the dynamic web application by navigating to URIs that were designed using REST principles via links. Users can create events which themselves have associated tasks. Users are then able to manipulate the name, date, and details of events as well as the name and details of the associated tasks. The application not only allows users to manipulate data in the database, but also constrains users access (edit and delete capabilities) to events and tasks that they own. This is done via user authentication when the user signs in. However, users can still view events and tasks of other photographers by navigating to an index page. This creates a collaborative feel and is similar to some designs of social networking sites. The integrity of the data in the database is further bolstered by the use of input validation. This ensures that incomplete or unidentifiable/irretrievable data is stored by users.

It is currently available on GitHub at [https://github.com/Hector-Ariceaga/evassist](http://). 

As of now, I am working on an additional feature that would allow users to use checkboxes to track their progress in completing tasks. The goal is to persist the state of the checkboxes, so that users can navigate away from an event's tasks and return with their progress saved. My plan is to save the state of a task, as a Boolean variable that belongs to an instance of a task, in the database. I will update this post if I get that feature working.





