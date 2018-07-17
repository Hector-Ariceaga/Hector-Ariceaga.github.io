---
layout: post
title:      "Designing a CLI Gem: Part II"
date:       2018-07-17 19:11:14 +0000
permalink:  designing_a_cli_gem_part_ii
---

A few weeks ago I completed my first CLI Gem and I am incredibly pleased with the outcome. I completed a code review. I was able to refactor my code into a polished finished product, and I managed to get the gem published on RubyGems.org, which was very simple and can be done almost entirely in the command terminal. As mentioned a few posts ago, I wanted to briefly review this process and discuss my thoughts.

## The Code Review
Honestly, this was the hurdle I was most nervous about clearing. It wasn't that I wasn't confident about my understanding of the project, my code, or the concepts. Rather, I was concerned that my excitement and eagerness to answer the questions and demonstrate what I have learned so far would cause my mind to run faster than my mouth causing me to speak too quickly (something I have rightfully been accused of more than once). I wanted to treat this like a true technical interview. FlatIron informs students ahead of time that this process is designed to be intentionally rigorous so as to better prepare you for future interviews. I completely agree with this approach and was eager to get my feet wet. I am no stranger to interviews, presentations, or speeches. I took the time to review my code myself, organize my thoughts, and determine positions for why I chose to design and write my code the way I did. I believe this prepared me for the interview and made it a much more pleasant experience. 

The interview itself was an absolute blast. I had a chance to describe each concept in detail and how the various pieces of my code interacted and referred to one another. I had a chance to describe my inspiration for the project and how I arrived at the solution that I did. It was also a great way to review the fundamentals, which I will continue to be building on going forward. My favorite part though had to be when we did some living coding. My interviewer made some suggestions for cleaning up my code. This was exactly what I envisioned collaborative coding to look like. She made some suggestions and I immediately got to work implementing them. Our time ended up running short but I was informed I did well and could continue on with the curriculum. Being the obsessive person that I am, I kept working on the suggestions and completed the refactor even though it wasn't required. I love how seamlessly I was able to incorporate my interviewers suggestions! 

I really wanted to go into the interview with an open mind and remind myself that any critiques or guidance I was given was only to further my understanding. I reminded myself not to get my feelings hurt if they took apart my code, because that is how a person learns, by finding where they are struggling most. I have to give a big kudos to the FlatIron staff that made the interview a challenging but entirely pleasant experience. I learned at least as much from the interview as the project itself, if not more.

## The Final Product
I was deliberately vague a few posts back about the topic of my project and the exact steps of development because I wasn't sure of the final direction my project was going to take and I don't want to spoil the learning process for future students. However, now that the project is complete I am happy to share the final product and explain how it works. I will still be abstaining from going into detail about the design process to protect the design of the assignment. 

What I have developed is a Command Line Interface (CLI) Gem that lists the newest property listings in Eugene that have been posted to Trulia. It can be installed traditionally by adding it to the application's Gemfile `gem 'new_eugene_listing_cli'` and executing `$ bundle` or by installing manually `$ gem install new_eugene_listing_cli`. Once installed, the user simply executes `$ new-eugene-listings` to begin the application.

The gem will scrape all of the property listings in Eugene, OR that have been posted to Trulia for less than 24 hours. It will then display them in the following format:

```
#. Address of Property -- Price of Property -- # of Bedrooms -- # of Bathrooms
     URL of listing on Trulia.com
```

The user can then type the number of the listing they would like more information on, relist the properties, or exit the application. The application accounts for edge cases and improper entries from the user. Once the user enters an appropriate listing number, additional details about that property are displayed in the following format:

```
Address:
Price:
Bedrooms:
Bathrooms:
Square footage:
Property type:

Description of the property
```

The user can then enter another listing number and go directly into more details of that property from this view or they can type `$ listings` to relist the properties.

There is also some minor ASCII filler to organize the information and make it more visually pleasing.

Part of what makes me so proud of this gem is that it scrapes the site each time it is executed, but after it is executed it will not scrape the site again, even if the listings are displayed again. This keeps the CLI application quick, responsive, and efficient. I'm also proud of the initial filter for only specific properties and the specificity of the selectors I used in the scraping. The Trulia site is full of JavaScript, so it was a fun challenge.

## Looking Back and Thinking Ahead
Looking back I don't know if there is much I would have done differently. I felt the project was an appropriate level of difficulty and enough of a challenge to push my learning forward. I didn't get stuck on any one part of the project too long because of the prep work I did ahead of time. One resource I utilized, will likely utilize again, and would highly recommend to others is using Trello to map out the project and the various steps. This allowed me to stay focused as I was moving forward. I was also able to document and save some ideas for exciting new features that could be added to this application if it was extended going forwarded, such as allowing the user to select the area in which they were interested in seeing properties from, allowing the user to select the age of the properties they wanted to see, and including additional details of the property.


