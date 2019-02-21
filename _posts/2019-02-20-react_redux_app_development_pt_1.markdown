---
layout: post
title:      "React/Redux App Development Pt. 1"
date:       2019-02-20 15:40:11 -0500
permalink:  react_redux_app_development_pt_1
---


I'm currently working on another passion project and I'm very excited about the process. I'm about halfway through meeting the initial goals I set for the application, so I wanted to take the opportunity to discuss the process so far, while it is still fresh. 

CDC-Spotlight, or Spotlight for short, is the application I am currently developing that displays articles and allows users to comment and contribute to discussion about the content in those articles. The ultimate intent of the application is to solve the problem of having to sort through articles manually to find the most valuable or relevant information. Specifically, this app is designed to work with the Centers for Disease Control and Prevention API and allow medical providers to review medical literature for the most up-to-date studies, frontline treatments, and standards of care. However, I wanted to expand on this slightly and allow users to post articles of their own and further contribute to discussion. To achieve this, I decided to build an API backend on rails that would handle the requests for user generated content, such as articles and comments. This then would be connected to a modular front-end that would allow the user to interact with the content within the Rails API, as well as the CDC's API. 

Why a rails API for the backend? 
1) This application should stick to some standard functionality, so flexibility down the road shouldn't be much of a concern (flexibility is sometimes  noted as a potential issue for using the Rails framework in development) 
2) It is very quick to launch and lends to declarative code, which can help with future development.

My hope is that since I am sticking to straightforward CRUD actions, at least in the beginning, that there shouldn't be any significant performance issues later if I do add more features.

I started by setting up the Rails API first so that my front end would have seed data to work with later. I wrote some RSPEC tests to test the functionality of the CRUD routes and FactoryBot to generate some fun generated data for the tests. After completing the basic routes, I wanted to start with for articles and comments I moved to the front-end.

For the front end I decided to use React. This will allow the front end to be broken into nice clean components that can be well organized. The virtual DOM that is used in React will also help speed up the application by rendering only the updated parts of the page, which should make for a better user experience. Furthermore, if I do want to scale the app upwards later down the line, implementing testing will be easier.

Shortly after starting, I recognized that React alone wasn't going to be the most efficient way to set up the front-end of the application. Some of the components were nested and I was having to pass state through several components down as props to get it to the components that needed it. Redux to the rescue! I decided to move my state to an external store and use Redux to connect to just the React components that needed access to state. This also makes it much easier to organize the code into containers and components so that it is clear at a glance, which components contain state, and which are purely presentational.

As of now I have my front-end and back-end communicating. I am able render a list of articles, but still must set up the individual show page route. I want the application to be a single page application, so routing is being handled on the client side using React Router DOM. Once the individual article is being rendered. I will have the comments render alongside, as well as form to enter new comments. I haven't connected the front end to the CDC API yet, but that will come shortly before I start working on the styling, which I plan to do last.

In the next post, I will be reviewing the specific goals I had for this application, how I met them, what I struggled with, how I overcame some of those challenges, and where I would like to see the application go next. 

![](https://www.freeiconspng.com/uploads/to-be-continued-meme-right-arrow-png-image-5.png)

