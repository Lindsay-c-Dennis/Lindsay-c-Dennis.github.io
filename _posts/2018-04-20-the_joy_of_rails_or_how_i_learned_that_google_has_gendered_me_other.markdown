---
layout: post
title:      "The Joy of Rails, or: How I Learned that Google Has Gendered Me "Other""
date:       2018-04-20 18:56:32 -0400
permalink:  the_joy_of_rails_or_how_i_learned_that_google_has_gendered_me_other
---


Another day, another portfolio project!

But this one - well, it was a doozy. As I approached the last portfolio project in Sinatra, I remember having this moment toward the end of one of the comprehensive final labs where I felt like I had turned a corner, like I had achieved a sense of mastery over the material. As I moved toward the end of the Rails section, I was optimistic that the same thing would happen with all of this new material. Optimistic, but a bit skeptical.

About a dozen lessons before the portfolio project, I was feeling like I had a decent grasp on the bits and pieces, but I had very little confidence in my ability to wrap it all together. I wasn’t too nervous yet - I had some big labs to tackle before I had to worry about the project. Then, one day, I woke up and I was suddenly on V5 - and only two lessons away from the project.  “I’m not ready!” I cried. My cats quietly blinked in response, simultaneously expressing their sincere confidence in my abilities and their total lack of understanding of even the most basic details of my situation.

There was nothing else to do but try. So I tried. And I failed. So I started a new project! That one failed, too. But the third? That’s the one I’m ready to hang my hat on.

My app is called the New Orleans Vacation Planner. As a former pedicab driver and tour guide (ie professional nerd), I had learned the ins and outs of the city’s history and tourism industry, so this seemed like a good fit. The site allows users to create a profile, browse local landmarks, and add interesting landmarks to their personal itinerary. Users have the option of leaving reviews of landmarks, and reading reviews from others. If a user is familiar with the city, they can mark themselves as a guide, which allows them to edit and create landmarks, neighborhoods, and categories. 

The basic structure was relatively straightforward, but I ran into a few stubborn hangups during the later stages of development. One of the biggest struggles I had to work through was getting omniauth to work properly.  Omniauth is a gem that allows users to use third party credential to create and sign into an account, to avoid having to create new accounts for all sorts of different sites. I had practiced using this gem through facebook, but their recent decision to only offer keys to apps with SSLs, while completely valid and prudent (given all the recent bad press), made my life more difficult. So I decided to use google.

The readme for the omniauth-google gem was relatively easy to follow, though it oversimplified the process of obtaining a key and secret from the google developer site, and did not really address the appropriate protocol for hiding this delicate information. I followed the example from the learn.co lab and hid it in a .env file. I had a bit of a learning curve figuring out how to use the auth hash from google in my sessions controller. I wound up using raise debugging to take a closer look at the hash being passed through, which showed me all the info about me that google was passing around, including a curious field in which they had apparently decided that my gender is “other”. Huh. Fair enough. Honestly though, it’s equally possible that sometime in the way-back-when (2007?) when I created my gmail account, I told them I was ‘other’. Google doesn’t need to know my gender anyway. I don’t trust them with it.

Beyond the gender curiosity, the main takeaway from examining the oauth hash was that my name and email were nested in another hash called ‘info’. This was crucial to making the feature functional. Finally logging in with google was a personal victory for me. There was much chair-dancing.

This project was a much more substantial undertaking than any of my previous forays into programming. The last two portfolio projects took 2-3 days; this one took me a solid week. Though I was initially very frustrated about how slowly I was progressing, at the other end of the tunnel, I’m happy with how it has worked out. I feel much more confident in my understanding of Rails, and I’m already considering other projects I’d like to tackle. This weekend, I will use my skills of a tour guide to create a bunch of seed data, and then I’ll make my first attempt to deploy with Heroku. Fingers crossed!

