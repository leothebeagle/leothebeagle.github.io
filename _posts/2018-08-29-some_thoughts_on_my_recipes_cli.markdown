---
layout: post
title:      "Some thoughts on my Recipes CLI"
date:       2018-08-29 04:55:33 +0000
permalink:  some_thoughts_on_my_recipes_cli
---



Starting off with a blank slate was the hardest part. Exciting, but with no code to build on or refactor it’s a little difficult to determine what a good starting point will be. 

I had a good idea what the individual classes I would need to build were, so I started off with a piece of paper and drew out as much as I could and started writing down how I envisioned the dependencies would be. Once I had that, I could then stub out on paper the various methods that I would need.

I knew that I wanted the responsibilities of my classes to be clearly segregated. My Recipe class would be responsible for instantiating new methods and my Scraper class would only be responsible for scraping data. With that in mind, I set out to build a scraper class with methods that would return hashes of data. 

The idea was then to define methods in the Recipe class that could iterate over these hashes to extract the data and instantiate objects and assign various attributes. I knew that this wasn’t how the instructions for the project specified we go about this, but it was code I felt comfortable with and something I could write to then refactor down the line.

Luckily, the website I scraped was very well organized, and scraping was a breeze. I built out my scraper methods to scrape the main recipes page to get data like name, description, and recipe url. I then built out another scraper method to scrape individual recipe pages to get more information like ingredients and directions on how to prepare the dish.

The bulk of the work was in combining my classes in the cli controller and in determining the flow of user interface. 

**Word of advice:** Review how to set up file structures and dependencies. I spent a good amount of time building out file structures and dependencies and then deleting them and starting over until I made sure I got it down. This saved me a lot of time during the actual project. 

