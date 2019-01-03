---
layout: post
title: Setting up Weechat & Vim as an IDE
date: 2019-01-02 22:13 -0500
---
# Intro

Just a small introduction, the school term was very busy and I slacked on this blog. But this past month has been very busy with this system I've been working on.
I never could imagine how much time I would sink and am still sinking to this day from setting up another Arch desktop system and actually using it as my main system. Though I had way too many headaches from trying to get i3 to stop producing artifacts/glitches. I've already given up after a week of trying to fix it and get help and gave the appropriate bug reports, I might visit it later.

I just got the blog up and running on this system where I wish I had it earlier to document my progress. Since I don't remember everything I did, I will blog of what I did today and onward.

# Weechat
I was recommended Weechat for its amazing multitude of features for a terminal IRC program. I thought it would be easier than IRSSI, but I very wrong. The first thing I did was get vimode so I could have vim bindings on it since the default bindings were terribly un-ergonomic.
However, this made the whole learning process much harder since I couldn't follow the userguide for weechat on the keys needed to be pressed to change settings and whatnot. I eventually found out that Weechat has its default keybinding in `Insert` mode of vimode.

Apparently Weechat doesn't support exporting and importing themes, also that it's a very bad idea to edit the config to change things around and you should be using `/set` and `/fset` inside the client. Pretty disappointing.
Luckily I only had to change some colors to make it less painful for my eyes to read certain things and notice other certain things better.
The statusbar for Weechat was very cryptic and I found out what it meant from `\fset` on its status.bar.items I believe it was called. I was given this link of an image that explained what each color setting are for.
![](http://anti.teamidiot.de/static/nei/*/Code/WeeChat/weechat-color-settings.png)

Another plugins I got was `autojoin` so I could save what channels I was viewing at the moment and have them set to be autojoined just by 1 command.
The window feature is definitely incredibly nice to have, though I think of them more as panes than windows as they're not X11 windows but instead similar to tmux panes. But I still need to figure them out. I need to figure out how to move to each window by number instead of using vimode's movement keys `Ctrl+W hjkl`, it's a lot of wasted movement I find. I'm surprised there's no given functionality or keybinding for changing the window size and zooming in on a window.
These are on my TODO list now as I need to make my speed better with weechat. I still fumble too much accessing windows and such.

I also changed weechat to be more compact by adding aliases that toggled long nicks, timestamps, and aligning, very very nice to have. I also eliminated the whitespace from the buflist. This was all needed so I can see from all 4 windows without it looking too tiny.

There are more miscellaneous changes I made but I will cut it short here, onto Vim.

# Vim

I haven't actually changed much of Vim today, but I was mainly researching on how to make Vim less painless when browsing and reading other people's code and projects. I remembered suffering quiet a lot when reading `top`'s sourcecode. I relied heavily on grep and then manually scrolled and read each line. I also tried reading the code of an open source game called One Hour One Life where where its draw function was something like 1,000 or 5,000 lines long. This is all very painful when you're not using useful IDE features like contract/expand and goto definition functionality. 
BUT VIM IS ABLE TO DO THIS FUNCTIONALITY. Unfortunately you need to plugins and quiet of bit of vimrc options.

The contract/expand functionality is doable through Vim's fold functionality. Though I still don't quiet understand it since the foldmethod being set to syntax still didn't work for a javascript file for node.js that I was testing on. I needed another setting, `let javaScript_fold=1`. Even now I don't know why this is required and I still need to find out how to make this work with other languages. Another day then.

The goto defintion functionality is apparently all done through third party plugins and isn't in-built to Vim, probably because it would make Vim very _BLOATY_. I've yet to try these but two main plugins that seem to be popular are CTags (and other versions of it) and YouCompleteMe (though this one is very bloat and complex). Another day also for this.

The only plugin I ended up getting was something to make my buffers show up as tabs at the top. <https://github.com/fholgado/minibufexpl.vim>  
I did not like this at all for my i3 setup. It took up a pointless bar of saying "My Plugin Name" essentially and it didn't seem to contract the tab name when there was overflow. Ontop of that the install instruction weren't there so I had to look elsewhere. (yeah I'm not super good with Vim)  
I will keep the plugin for now since I'm probably just using it wrongly.
