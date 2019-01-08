---
layout: post
title: Git submodules, tmux, and alacritty
date: 2019-01-07 19:56 -0500
---
# Dotfiles and git submodules
As of lately I am keeping a dotfiles repo for my desktop. It never occurred to me before as I thought I can easily transfer stuff with a USB stick, but it's better to get it done with early and just version all your important dotfiles so you won't be bothered with it later. I already lost my vimrc files and such due to laziness of backing up. Dotfiles are typically configuration files. In mine I tastelessly use both systemwide configuration files and user configuration files. I dislike when my aliases, themes, and whatever don't transition when I'm logged in as root when I'm doing maintenance so I tend to use systemwide configuration files for this personal computer of mine. Though I was told that `sudo -s` should make it so your regular user's settings and themes work when you're root? I have to confirm this sometime later.

I found out later that git doesn't like it when you git clone inside it. Why I did this? So I can have my fancy themes and plugins where they can be up to date if I desire. However when adding these directories with their files, vim complained that these need to be added as submodules or something along that line. This was quiet the headache since I had to deal with ten git clones.

What I ended up doing was using `git submodule add` for one of these directories which in turn added an entry to a `.gitmodules` file in the base of the git repo. Then I proceeded to do everything via the `.gitmodules` file using that example since I found this faster. Using `git submodule add` for already cloned repos seemed to cause problems.

On a sidenote, I am glad that git supports symbolic links like ../git/whatever.

# tmux and alacritty
I never knew how nice tmux could be until now. I thought it would only be useful for ssh sessions or such. Since I switched from bigger and fuller terminal emulators like from gnome-terminal to more lightweight ones like urxvt, I needed a way to group related terminals together. I first tried doing this via my WM, i3, where it has a tabbed layout featured. I found this more annoying than it was worth since I was always moving around these terminals tabs and out of them. I prefer having keybinds separate for terminal tabs and general X11 windows. So I listened to a recommendation to use tmux for tabbing terminals. I was hesitant at first because I thought tmux looked ugly but then I switched to nord-tmux (<https://github.com/arcticicestudio/nord-tmux>).

This actually looked better than gnome-terminal in a way. However I found out that urxvt does not use fallback fonts so this plugin/theme really messed with urxvt. I decided to switch to another terminal emulator that does like to play well with fonts which was alacritty. I actually prefer it now since proper clipboard functionality works right out of the box.

If you like to use vim, then you better rebind your tmux prefix and such. Unfortunately vim and tmux conflicts with keys, specifically ctrl-b which is the default prefix for tmux to do tmux commands and it's a scroll full page back command in vim. Even now I see some odd things happening in my tmux which I have no clue what's happening. Like these arrows at the end of each tab here.

![]({{ "/images/2019-01-07-git-submodules-tmux-and-alacritty/1.png"}})
