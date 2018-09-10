---
layout: post
title: Creating a website
date: 2018-09-10 00:48 -0400
---

For the past month I was very busy and was also working on a website. I am making almost everything from scratch for the experience, the front-end, the back-end, the database, and setting everything up on a VPS. I won't get into the specifics, but there have been many things I've been learning from CSS alone. This won't be a long post as it's fairly late, but I procastinated this blog long enough.

I implemented everything via node.js, bootstrap, and jquery. There's some various middleware and general node.js setup, but for now I am focusing on the front-end. I doodled up a mock sketch of a front page and asked if it would be good, it was and then I just tried to mimick it.

Bootstrap doesn't have that many features as I thought it would. Some features that I needed to get from elsewhere was a slideshow/carousel showing multiple slides at once, and a ken burns carousel for the banner.

I was surprised how advanced CSS was and how it was so versatile, especially when messing with animations and making the page compatible with lower resolutions.

One of the things you have to watch out for in Bootstrap is the numerous options it offers and you really need to be on the ball with understanding it. For instance the grid system, I was very careless with it. For instance the col-* sub classes like sm, md, and lg. Also that it you want to specify how many columns of the 12 you want to take up. Even now I don't know how it should be properly utilized for building a multi-platform site that is optimal for desktops and mobile. I ended up just doing hackish fixes with CSS utilizing @media tags and checking the width of the window.

A pit fall I notice was utilizing the unit measurement `vh` in CSS. For instance if you had developer browser tools open or the search bar open, it messed up the page's initilization where if you closed those there were gaps. I can see why `em` is used more.

I found that I just did a lot of customization via CSS, which I am doing through Stylus, a stylesheet language. It's a bit of a hassle where I have to have a terminal open that watches changes in my .styl file and compiles a .css file.

I will have to end here for tonight. There's too many steps and problems to detail, and unfortunately I did not blog about them. I might create shorter blogs to address this as I'm still figuring out how to blog.

Here's some screenshots of the website. I still need to choose a good color scheme and most other aesthetics aren't done either.


![]({{ "/images/2018-09-10-creating-a-website/1.png"}})

![]({{ "/images/2018-09-10-creating-a-website/2.png"}})
