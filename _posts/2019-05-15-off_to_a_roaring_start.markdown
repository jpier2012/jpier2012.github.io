---
layout: post
title:      "Off to a Roaring Start"
date:       2019-05-15 15:24:30 -0400
permalink:  off_to_a_roaring_start
---


I just finished my Beautiful Billboard CLI project, and, I must say, it was a great experience! 

If you haven't yet checked it out, you can find the repo here: https://github.com/jpier2012/beautiful_billboard.

It's a basic CLI app that pulls info from the Billboard Artist 100 and Hot 100 charts (https://www.billboard.com/), and looks great while doing it!

Through the project I learned a ton and this was a great inspiration to get creative with code. Below are some bits of wisdom that I discovered through working on this project... and by "discovered", I mean I had read about these aspects of development prior to starting the project, but hadn't witnessed them firsthand until now.

1) It's more important to write code that's readable by humans and easy to maintain, than to find clever ways of writing fewer lines that may obscure the code's intent. During this project it was tempting to try and use the "shortcuts" I've seen throughout this course, but these time and space savers (e.g. ternary operators, using default initialize conditions) can sometimes not look as clean and organized as other alternatives, which make the code harder to work with. Ultimately, computers are extremely fast nowadays, so performance is not likely to be a concern with most apps.  A classic example is FizzBuzz (https://ditam.github.io/posts/fizzbuzz/) - There are countless ways of achieving the same end but, as far as the end user is concerned, they're all going to get the job done equally well. Web app performance seems to only be significantly impacted by large-scale inefficiencies, not small nuances. It's really up to you to determine the most comprehensible solution because you'll be the one(s) maintaining the code.

2) Code comments make the world go round. They only take a moment to write and can save you tons of time when reviewing code in the future, even if it's just you looking at it the next day. Prior to joining this bootcamp I had written some VBA for my job, but had never left comments because I was the only one looking at it. Oh, how much time (and confusion) I could have saved by keeping notes on my work!

3) Similarly, making meaningful and well-labled git commits is very important for keeping your work organized on a large scale. With the next project I will need to be more clear and consistent as to what changes I commit and how they're labeled.

4) Ambiguity can lead to over thinking(!). Sometimes we need to make decisions without having all the information we need, and we shouldn't spend too much time trying to find answers to low-priority questions (e.g. 'what color scheme is most appropriate?'). This project specifically was fun because it was my first chance to "show off all the new tools in my belt" so to speak, with clear requirements on what the program needs to do, but without specific instructions on how to do it.

So, since I'll be showing this program to potential employers, I wanted to have some fun with it and make it challenging, which is part of the learning process in and of itself. This meant asking myself questions while fleshing out the app design:

* Should I create Album objects from the Album 200 to relate to the Artist 100?
* Should I show the user a list of Hits? 
* Do the Hits even need to be their own objects, or just strings to collect and print?
* Should I include links scraped from a Google search of the artist?
* Which data would look best in a table?
* How practical does this project app really need to be?
* How many bells and whistles do I want to cram into this before I call it 'done'?

Although it could have been fun to spend more time building the app, I decided it was time to submit the project and move on before I make it more complicated, solely to prove that I can. This first CLI is meant to be a benchmark of progress more than anything, which it surely is, and I must say I'm proud of the work I have done.

I'm already looking forward to the next assignment!
