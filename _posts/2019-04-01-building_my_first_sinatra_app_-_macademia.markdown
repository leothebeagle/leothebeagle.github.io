---
layout: post
title:      "Building my first Sinatra app - Macademia "
date:       2019-04-01 16:16:24 +0000
permalink:  building_my_first_sinatra_app_-_macademia
---


Having completed the Sinatra section, it's now time to build a simple Sinatra webapp. 

The requirements generally focus on our ability to implement (There are other requirements not included here):

	- A Model - View - Controller.
	- A has_many relationship.
	- Associations among tables in the database.
	- User account creation and login capabilities.
	- user input verification.
	- Flash messages
	- Forms and the routes that process user input.

The site I chose to build is an online school. A student can create an account and create programs of their choice that have different selections of topics. A student is able to select a program name and from a list of topics,  select those they want to include in the program. 
	

I first set off to build the models and establish the associations amongst the tables:

A student has many programs
A program belongs to a studentEach program has many topics
And a topic belongs to many programs

The key in associating topics with programs is a topics_programs join table.

Building the Sinatra routes and views was relatively straight forward, once I had a basic flow working with views and routes operating as I wanted them to, I added some conditional logic to some of the views. For example, I wanted to only display the login and create users buttons on the home page if a user wasn’t already logged in. I achieved this using erb in the view file.

A pretty useful tip I learnt making this app, is to display certain things in the view based on conditional logic. I explores this concept further by combining it with error messages, in the form of Sinatra Flash.

The idea is that if a certain condition isn't met when the route receives user input, for example a missing username from a login form, then a flash message will be set.
In the corresponding view, I set up a conditional statement that checks to see if a flash message was generated. If it was, then an error was made, and so a specific error message and link is displayed. (ex: a link to go back to programs, or to login.)

