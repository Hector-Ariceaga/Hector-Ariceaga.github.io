---
layout: post
title:      "Rails + JS: Breaking Down the Development Process of Theurgy V2"
date:       2019-01-13 20:40:19 -0500
permalink:  rails_js_breaking_down_the_development_process_of_theurgy_v2
---

### Goals
For my most recent project I decided to build upon my previous rails application, Theurgy. For the past few months I have been practicing writing vanilla JavaScript, jQuery, working with APIs and JSON. Applying these skills and technologies to an existing fully functional application seemed like a great way to get additional experience and expand my understanding. Because vanilla JavaScript alone presents a plethora of opportunities for expanding functionality I knew there was going to be far more features that I wanted to complete than I had time to devote for this one project. As such, I set a few concrete goals to guide the process, while still leaving the code in a state that could be picked up and further extended in the future. My main goals were as follows:

1. Utilize Active Model Serialization on the back-end to return JSON that could then be used to render an index resource (using JavaScript and jQuery) without requiring a page refresh.
2. Utilize those same technologies and techniques to render a show page for a specific resource.
3. Render a resource that has a 'has many through' Active Record relationship dynamically to the DOM using a combination of JavaScript and AJAX
4. Use JavaScript and AJAX to submit a form and render the returned JSON in a show page for a resource without requiring a page refresh

### Development Plan
1. Add necessary jQuery and Active Model serialization gems
2. Customize serializers to gain access in JavaScript to resource attributes through relationships and methods
3. Create and bind event listeners to elements in the DOM to trigger desired functions, using a combination of JavaScript and jQuery
4. Create AJAX requests to send GET and POST requests to server
5. Refactor controller methods to handle JSON requests and return JSON response
6. Create class constructors in JavaScript to build resources
7. Add prototype methods to classes to format attributes and generate ES6 Template Literals
8. Update DOM

### Hurdles and Road Bumps
Initially, I thought about building the JavaScript and API back-end modularly. I took the opportunity to build a database in PostgreSQL, which I hadn't had experience working$(document).on('turbolinks:load', function() {
 with previously. I built out the entire API back-end in one repository and then created a repository for the front-end and JavaScript. I set up adapters and had the front end make AJAX requests to the API back end. The front end then created JavaScript objects and Template Literals to update the DOM. While this worked, I realized that this didn't allow for HTML responses. I spent several hours setting up the modules, and while it mirrored Theurgy it wasn't building on the previous application like I set out to do. Ultimately, I decided to go back to my original Theurgy application, create a new branch, and add the JavaScript functionality in that branch. Although it was a circuitous route getting started it gave me an opportunity try building a modular front and back end as well as work with PostgreSQL.

A few other hiccups I encountered along the way were:
1. Working with Turbolinks - I had to ensure Turbolinks was loaded, after the DOM was loaded, before I could bind the event listeners. If I didn't do this, when Turbolinks would refresh only part of the DOM the event listeners wouldn't be able to find the elements to bind to.

```
ex: $(document).on('turbolinks:load', function() {}
```

2. Resource methods need to be set in the serializers - I found that the class and instance methods defined in the models aren't accessible to the JavaScript constructor, but if they are added to the serializer as methods that return objects then they can be used.

```
ex: class TherapySerializer < ActiveModel::Serializer
  attributes :id, :patient_id, :treatment_id, :active

  belongs_to :patient
  belongs_to :treatment

  def patient_name
    {patient_name: self.object.patient.name}
  end

  def treatment_name
    {treatment_name: self.object.treatment.name}
  end
end
```

3. Working with Template Literals - These can be tricky and having large strings of HTML with interpolated JavaScript is messy. Using broken out methods and smaller templates within larger templates makes the code more readable, maintainable, and easier to debug.

### Successes and Takeaways
By the end of the project I was able to meet each of my goals, as well as add some extra features. I met each of the goals in the following ways:

1. Set up treatment links to render an index of treatments without a page refresh
2. Set up treatment details links to render a show page of treatment details without a page refresh
3. Added the ability to select and assign a therapy and update the patient show page with the new therapy appended without a page refresh
4. Refactored new patient form so that upon submission a preview of the new patient that was added is rendered in the DOM without a page refresh
BONUS
* I refactored the new patient form so that symptom and diagnosis selection is from a list, rather than the less pleasing Ux of interacting with a ton of check-boxes

I spent much longer on this project than I had initially intended, but this was largely due to the exciting number of new features that could I implement or improve on with JavaScript. In the future I am hoping to extend the dynamic rendering of resources to the symptoms and diagnoses. I would also like to get the edit form to render the patient show page dynamically. Ultimately, a user could navigate throughout the whole site once logged in without a page refresh being necessary. I'm sure there are more effective and cleaner ways to address the features I worked on in this project. I am looking forward to using those methods in further improving Theurgy in future iterations. 


