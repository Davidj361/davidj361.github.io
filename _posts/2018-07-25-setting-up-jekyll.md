---
layout: post
title: Setting up Jekyll
date: 2018-07-25 13:31 -0400
categories: development
---

Now this was quite a difficulty. Especially when you're working in a Windows system as it's "unofficially supported". I ended up going through 10-20 links searching for a tutorial on publishing a blog to Github and creating it on a Windows environment. Most of them just explained how to generate files with Jekyll, etc, but I had no idea how Github served the website. Was it through the generated files in `_site`? It was very confusing considering as I had my repo folder as well as the folder created from `jekyll new`, also I had no idea what `jekyll build` and `jekyll serve` generated. Most of the tutorials assumed you knew what to do with all the files generated.

I first made the repo under _githubUserName_.github.io as was said on Github Pages' help guide. It was very confusing if this was wise or I should've made a repo named blog and used a `gh-pages` branch along with other possible ways as I viewed from youtube and other tutorials.

So I decided to first get Jekyll and follow their official documentation on installation. Went to <https://jekyllrb.com/docs/windows/> and ignored the Windows 10 section as I didn't notice it until I finished the first part. As for downloading Ruby+Devkit, I also installed MySys2 as I wanted a Linux environment. I ended up using `Start Command Prompt with Ruby` as the tutorial said I needed to use PATH environment variables.
I followed most of the steps up to something relating to using UTF encoding: `chcp 65001`
I ignored the section on TimeZone Management as I didn't see a problem regarding dates and times.

Seeing that this wasn't tailored completely to Github, I ended up searching for another tutorial which lead me to this <https://help.github.com/articles/setting-up-your-github-pages-site-locally-with-jekyll/>.

Skipped to step 3 as the repo was already setup and that Jekyll was already installed. An odd thing about step 3 was that it was telling me that `"jekyll", "3.3.0"` should be removed, but I did not see it in my `GemFile`. Unfortunately I have to use `bundle exec jekyll serve` instead of `jekyll serve`, the reason being is that `bundle exec jekyll _3.3.0_ new NEW-JEKYLL-SITE-REPOSITORY-NAME` made this a different version of Jekyll than the global Jekyll as was told to me by _allejo#jekyll@freenode_. He said it's ok to use a different version since Github is Jekyll's parent.

Another gripe I have with this blogging system is that even the posts in `_posts` weren't generated out of the box by date & time. Which made me ending up installing another Gem (a plugin) that generates the post files for me (<https://github.com/jekyll/jekyll-compose>). Thankfully this Gem exists (no pun intended *wink*), but I don't see why that's not an out of box feature already. It's a bit odd that you are tasked with editing things like: `date: 2018-07-25 13:31 -0400`.

Which about wraps up how I got this blog setup.