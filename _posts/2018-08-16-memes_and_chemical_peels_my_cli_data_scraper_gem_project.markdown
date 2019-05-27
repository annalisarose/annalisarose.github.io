---
layout: post
title:      "Memes and Chemical Peels: My CLI Data Scraper Gem Project"
date:       2018-08-16 17:16:55 -0400
permalink:  memes_and_chemical_peels_my_cli_data_scraper_gem_project
---


![](http://oi63.tinypic.com/2lnn7a.jpg)

<p>Assignment: "Build a Ruby gem that provides a Command Line Interface (CLI) to an external data source. The CLI will be composed of an Object Oriented Ruby application. The data provided must go at least one level deep. A 'level' is where a user can make a choice and then get detailed information about their choice."

<p>I decided to scrape knowyourmeme.com, one of my favorite and most-visited websites. It's basically Wikipedia for memes, an invaluable resource in this age of ever-evolving web humor. My goal for the interface was:

<p>* user runs the app
<br>* user is greeted by app, and then presented with a list of top memes and a menu of options: enter number of meme to expand, enter 'list memes' to repeat the list, or 'exit' to exit the program
<br>* if a meme is selected, return information on that meme
<br>* repeat the menu after all options except for 'exit'
<br>* if exit is selected, the app says "goodbye" and the program exits

<p>I finished up the first iteration of this app while I was at home recovering from a TCA chemical peel. For the unfamiliar, a chemical peel is a skin resurfacing treatment where a cosmetic dermatologist applies medical acid to the epidermis. The top few layers of skin dissolve, removing sun damage and softening fine lines. (It's pretty hardcore.) My technical coach, Ben, pointed out the parallels between the peel and my CLI scraper project, and I had to agree. I had scraped off a superficial layer of skin to reveal a glowing complexion, just as I had scraped through the HTML of knowyourmeme.com to reveal the top 8 most popular memes.

<p>This project made me realize that I'm really into modules and object relationships. Figuring out how to most efficiently delegate which tasks to which modules was as satisfying as exfoliation.
