---
layout: post
title:      "Give it a REST(Representational State Transfer)"
date:       2018-08-06 18:13:02 +0000
permalink:  give_it_a_rest_representational_state_transfer
---


Recently I have been working with Sinatra and Active Record. I just completed a simple playlister web application that allows a user to perform some basic CRUD actions via HTML forms on a database created from mp3 titles (artist - song - genre). During development I was also practicing RESTful routing. 

REST is an architecture, or pattern, designed by Roy Fielding that constrains how web services are created. These constraints help standardize URIs and other web services so that they can be more easily maintained, debugged, and extended. One of the keys to successfully creating RESTful content, and routes, is understanding which HTTP verbs are associated with which CRUD actions and which routes in a RESTful application.

Here is a quick breakdown:
* GET - used with a controller action to display content, it could be a show route (routing to just one item) or route to an index
* POST- used with a controller action to create content
* PUT/PATCH - used in two different ways with a controller action to update content
* DELETE - used with a controller action to delete content

The playlister application that I completed required using each HTTP verb in conjunction with a controller action and Active Record to display information from the database, manipulate information in the database, and delete information from the database. By using RESTful routing the controllers are lightweight and follow the single responsibility principle. Because the responsibility of each route is clearly defined, additional functionality can be added later.

One issue I ran into while working on this application was pages returning status codes of 500 rather 200. Initially, this seemed difficult to debug because the status code 500 is fairly vague. MDN web docs even states in their 500 Internal Server Error documentation "This error response is a generic "catch-all" response." However, I found that the majority of the time this error was actually the result of a simple error in my Ruby code, errors that I would typically catch thanks to TDD, like mistyped variables, mismatch errors, and the like. Once I had an idea of where to look seeing a status code of 500 returned became more of a reminder to double check my spelling and syntax.

