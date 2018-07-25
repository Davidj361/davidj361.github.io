---
layout: post
title: First time working with C# and creating a Space Engineers in-game script
date: 2018-07-25 17:05 -0400
categories: programming modding
---

I became interested in creating an in-game script for a game called **Space Engineers**. The language is in C#, which I never worked with, but I knew C++ so I knew I could scrape by. Programming for games can be quite fun as you get to see your results in a 3D/2D environment with not much code needed. However, the documentation for this game is very out dated. The official wiki still lists depreciated API which the game told me is depreciated where I made a wiki account to state it was depreciated <https://www.spaceengineerswiki.com/Programming_Guide/Action_List>. I was even told that the admin of the wiki has been missing for years, yikes. After asking the *#programming-in-game* channel in the *Keen Software House* discord server about proper API documentation, I was directed to [MDK](https://github.com/malware-dev/MDK-SE). I saw it at first both a blessing and a curse. The reason for it being a curse was that it was an extension for Visual Studio. I had used Visual Studio back when I first started learning programming and I remember the bloat it introduced. The horrible loading time, the bloated size of my projects from doing exercises in the book I was learning from; I still have these on my drive to this day. The *#programming-in-game* channel explained that the 2017 version isn't as bad and is free. In the end, the loading times weren't as bad (probably because of my beefy computer), but I still have bloated built projects in file size. For instance, an in-game script that's around 300 lines resulted in 50 MiB being created; I will have to fix that later.

Now the first problem was figuring out how to have a script pause at a point and resume. I was recommended in creating a [`yield` state machine](https://github.com/malware-dev/MDK-SE/wiki/Easy-and-Powerful-State-Machine). Having no experience with C#, I did not know what `yield` was. In short summary, `yield` is a magic keyword you use to return values to an algorithm/function running IEnumerable functions to see if there are still instructions to run or if that's it. The reason you want this is so you can tell the game what instructions to execute in 1 game tick. That way you can spread the script's processing among the game's tick without making it lag from executing too many complex instructions at once and dropping simulation speed in the game. They were confusing like goto statements. For example, when you see `yield return true` you would expect the function to end right there, but in reality it does not. Another struggle was figuring out how to run `IEnumerable<bool>` functions within one another. In both of these functions, you needed to use yield in your returns, e.g. `yield return true`. However, the function calling the other needed to call in a very strange way.

```C#
IMyTextPanel _textPanel;

public IEnumerable<bool> MainState() {
	// This is how you call IEnumerable functions
	foreach (var step in InitializeVars()
		yield return step;
	if (_textPanel == null)
		yield return false; // CRITICAL FAILURE
}

private IEnumerable<bool> InitializeVars() {
	_textPanel = GridTerminalSystem.GetBlockWithName("LCD Panel Test") as IMyTextPanel;
	yield return true; // Isn't needed as it by default it would yield break; by default
}
```

That's right, to call functions you have to `foreach` them due to the IEnumerable function type. If the function was not an IEnumerable type, you could use functions as you would normally.

Now the first thing I wanted to make was an airlock system, as this was a space game. It needed 2 sensors, 1 on each side to detect which door to cycle to, a vent, and the doors. It was very difficult working with the entities and debugging the script on a server because the game still had many bugs such as desync issues where to some clients the doors appeared closed or opened but you could phase through them, thus I had to switch to single player to finish the script. The next hurdle was the name scheme as I wanted numerous airlocks. I went with this name scheme: '[Airlock 1] [Airlock 1 In] [Airlock 1 Out] [Airlock 2] ...' I had to create an Airlock class that had doors, vents, and sensors where each airlock can be '[Airlock 1]', '[Airlock 2]', etc, depending how the player custom named everything in the game. To get this to work, I needed to use regex.

`string pattern = @"^[^\n\[]*(\[Airlock (\d+)(?: ([^\]]+))?\])";`  
<https://regex101.com/r/clAsfB/8>

Regarding the `\n` and such, that was just to deal with multiple lines in regex101. With this regex, we check if a match or group 1 exists. If it does, we look at its ID and make an airlock, or don't, and put this entity into the airlock. I managed to finish the script in a day after dealing with many bugs and issues, it was quite a learning experience.  
The end result: <https://github.com/Davidj361/airlocksystem>