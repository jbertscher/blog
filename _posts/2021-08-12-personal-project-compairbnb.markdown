---
layout: post
title:  "Personal project: Compairbnb"
date:   2021-08-12
categories: personal-project personal-record learnings
---

What follows serves to bookmark a project that I’ve been working on for a few months.

I’ve often found it difficult to coordinate booking holiday accommodations with friends. I think it’s because it’s difficult to keep track of what you’re considering, compare the features of all of the candidate listings, and ascertain everyone’s preferences. And I think it’s a shame. Some of my best times have been with friends in a holiday accommodation.

I recently came across the idea of [social shopping](https://en.wikipedia.org/wiki/Social_shopping), which got me thinking: if it were easy to see all the listings in one place and compare them with friends, recording everyone’s preferences along the way, then perhaps this problem could be alleviated.

## A solution

And so Compairbnb was born - an app that allows you to add Airbnb listings to a shared table so that you can compare their features. It allows users to add comments and submit their preferences for booking each property.

The app is live on Heroku [here](https://compairbnb.herokuapp.com/public). You can also see the code on the [Github repo](https://github.com/jbertscher/compairbnb/).

It’s at a stage where it’s usable, although it falls far short of its full potential. I’m keen to move on to other things right now so I’ve drawn a line in the sand, deeming “version 1” complete. It was always really more about learning and having fun than anything else. However, I might revisit this one day and take it to the next level.

**Disclaimer:** This app leverages a library that makes use of the unofficial Airbnb API. It could break Airbnb’s terms of use. Changes that Airnb makes to their API could also break this app without notice.

## Walk-through of basic functionality

The app has 3 main visual components:

- Input for a listing’s URL
- Input to enter the name of a user so that they can enter their preferences
- The table, with listings as rows and properties of the listing as columns

This is what it looks like:

![image1]({{ BASE_PATH }}/assets/images/2021-08-12-personal-project-compairbnb/image1.png)

One feature I think is particularly useful is its ability to parse out the number of each bed type specified:

![image2]({{ BASE_PATH }}/assets/images/2021-08-12-personal-project-compairbnb/image2.png)

You can delete listings by clicking on the red “x” on the left-most column. You can toggle column visibility using the context menu of the second column:

![image3]({{ BASE_PATH }}/assets/images/2021-08-12-personal-project-compairbnb/image3.png)

You can add comments:

![image4]({{ BASE_PATH }}/assets/images/2021-08-12-personal-project-compairbnb/image4.png)

You can delete users using a context menu:

![image5]({{ BASE_PATH }}/assets/images/2021-08-12-personal-project-compairbnb/image5.png)

You can sort columns:

![image6]({{ BASE_PATH }}/assets/images/2021-08-12-personal-project-compairbnb/image6.png)

I froze the first 3 columns so that you always know what listing you’re looking at when you scroll horizontally:

![image7]({{ BASE_PATH }}/assets/images/2021-08-12-personal-project-compairbnb/image7.png)

## Components

I built the site using Python and Flask, with some Javascript on the front-end. I relied heavily on the JS library [Tabulator](http://tabulator.info/) for the table component. I use the [unofficial Airbnb Python API](https://github.com/nderkach/airbnb-python) to attain the listing data.

## Improvements

While I’m happy to draw a line in the sand with this “version 1”, I might iterate on it in the future. There are many things that could be improved. There's an issue with writing long multi-line comments that exceed the space of the textbox and it can take a couple of clicks before it registers user preferences. There were also a few formatting niggles that I would fix.

There are a few things that I’d also add to improve functionality:

- Automatically aggregate user preferences to highlight the “most preferred” listing.
- Allow all the fields that are contained in the Airbnb response to become columns in the table, with an easy way to toggle them on and off
- Automatically show columns that highlight differences between listings.
- Have some way to create your own “trip” via the home page. This could be done by allowing users to create an account or just going to a page that generates a unique URL that you can use (the latter isn’t very secure though - if someone figured out your link, they could modify your trip).
- A Chrome extension for adding listings without having to copy and paste the URL.

Something that I would most like to change, but it would be a big one, is to pivot away from relying on Airbnb listings. As I’ve stated above, I’ve used a library that hacks Airbnb’s unofficial API. This may not only break that company’s terms of use but changes that they make - including requiring authentication to use the API - could break my app. An alternative could be to try to find some open API or sign up to an affiliate program with a booking company.

## What I learned

I learned, and relearned, a few interesting things:

- How APIs work to connect the back and front end of a website. I had to pass data back and forth between the JS frontend and Python backend.
- How amazing the Javascript off-the-shelf visual components can be. I was going to implement my own table component but this wasn’t necessary because I could leverage the Tabulator library, which did it better than I could have.
- Fundamentals of MongoDB, using PyMongo to save, modify, and retrieve data.
- How to set up my own web app on the cloud using Heroku to host the app and Mongodb Atlas to host the database in the cloud.
- Better coding practices. I refactored my code several times to make it more object-oriented, encapsulating trips and listings within their own respective classes. I used a debugger properly for the first time, which blew my mind. I implemented typing hints and linting to improve my coding style.
- I refreshed my memory on Javascript and basic HTML.

## Conclusion

This was a fun and rewarding project. And I learned a lot. Right now I’m keen to move onto other projects but I might revisit this in the future.
