---
layout: post
title:      "Aw, CRUD, it broke again..."
date:       2018-03-29 16:54:14 +0000
permalink:  aw_crud_it_broke_again
---


Here we are again. I made it through the CLI gem evaluation unscathed, pushed through SQL and ActiveRecord, and became well-acquainted with the 'ol crooner himself. I even reached detante with my co-dependent cat - he's agreed to stop pulling books off the shelf if I let him sit in my lap while I code. A state of relative piece and prosperity had been achieved. And now here comes this Sinatra project to throw a wrench in the whole process.

 ![](https://scontent-dfw5-1.cdninstagram.com/vp/252575da3740aeb48af4887c2f6f915e/5B6DDC2B/t51.2885-15/s640x640/sh0.08/e35/28754395_433684477076224_749151968412303360_n.jpg)
 *Todd does not appreciate a jittery lap*

Really, it's not so bad. When I landed on the CLI gem project, I was incredibly nervous, mostly because my knowledge of file structures and file requirements and git mechanics was largely theoretical up to that point. But the process of creating that gem demystified many of those aspects. Plus, in the interim, I have become very comfortable working on projects locally, after my occassionally spotty wireless internet spoiled my honeymoon with the browser Learn IDE.

I had been feeling generally comfortable with the Sinatra unit right up until the complex form associations lab. This lab is 50% walkthrough, yet still took me hours, and wound up being one of those labs I had to put on the backburner for a couple days and then come back to finish. And then that playlister lab? I spent an entire Saturday struggling with that! My computer-directed swearing scared the cat off my lap and now the entire apartment is strewn with half-open sociology texts and Margaret Atwood novels. Priority #1 after completion of this project is to restore order to the library.

Speaking of libraries...


I decided to structure my Sinatra app as a social reading list that allows users to create reading lists and reading goals, and to view lists of books and goals from other users. There are also index pages for books and authors, and show pages for books and authors. The author show pages include links to all books written by that author, and the book show pages feature a list of users reading that book, as well as a button that allows the user to add the book to their reading list (which is only visible if that book is not already on their list). The user's homepage includes their reading list and their list of reading goals. Reading goals can be created, edited, and deleted only by the specific user. Books can be edited, but not fully deleted (because books have a many-to-many relationship with users). The delete button simply removes the book object from that particular user. 

Overall, the process was occassionally frustrating and sometimes tedious, but I never got stubbornly stuck in the way that I did a couple of times while crafting my CLI gem. Perhaps this is coincidental, but I choose to attribute it to my consistently vastly improving hAcKzOrZ!1! skills. I did discover, though, that building CSS stylesheets from scratch is not my idea of a happy fun time. I definitely have opinions are site achitecture and user experience, but no real eye for design, which is pretty readily apparent to anyone who has ever visited my cat-filled, book-strewn apartment.

And now? Well, it all works! After working out a few little kinks. I had to record three separate video walkthroughs before I managed to walk through all of the site's functionality without stumbling upon errors. But, you know, Windows 98 crashed on its live TV debut, so I suppose it can happen to anyone.

Here's hoping the inspection goes well!
