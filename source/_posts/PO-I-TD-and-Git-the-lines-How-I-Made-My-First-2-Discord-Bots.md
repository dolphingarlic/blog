---
title: PO(I)TD and Git the lines - How I Made My First 2 Discord Bots
date: 2020-05-14 18:09:47
tags:
    - Bots
    - Discord
    - GitHub
categories:
    - Programming
    - Projects
---

I've recently made 2 Discord bots. [PO(I)TD](https://github.com/dolphingarlic/PO-I-TD) is a bot that sends a daily Olympiad Informatics (OI) problem and [Git the lines](https://github.com/dolphingarlic/git-the-lines) is a bot that prints out GitHub/GitLab snippets.

Both bots are open-source and you can see their code [on GitHub](https://github.com/dolphingarlic)

<!-- more -->

## PO(I)TD

PO(I)TD was my first Discord bot. The inspiration behind it was the *#potd* channel in a Discord server I'm in, which was a place where people could post daily challenges. The sad thing was that people didn't post there very frequently, so it was more of a problem-of-the-month channel than anything.

I also really enjoy solving OI problems. Before making this bot, I created [APIOIPA](https://apioipa.herokuapp.com/) - an **API** for **OI** **P**roblems **A**ssorted.

These 2 things got me thinking - what if I could automate POTDs *and* use this API? Of course, I immediately thought of making a Discord bot.

The bot was surprisingly easy to make. It only supported 1 command (`$gimme`) and it only had to do 1 other thing - get problems from my API. I hosted it on Heroku, added it to an OI server with some friends, made it give a problem every 24 hours, and everything went smoothly.

That is, until the third day it had to give a problem.

You see, PO(I)TD used to listen for `$start` and `$stop` commands to determine when it should start sending problems. This works fine if the script runs 24/7 and never restarts. However, Heroku restarts the script seemingly at random. This meant that I had to type `$start` every day for the bot to work, which was not ideal.

That's when I discovered `asyncio`.

It was like magic - my bot could delay one command for several hours while other commands ran during that time. Using this, I simply made my bot sleep until midnight UTC each time the script started before starting the 24-hour problem-loop. This meant that no matter what, a problem would be sent at midnight UTC.

That was a pretty fun project and a nice introduction to Discord bots for me.

## Git the lines

Git the lines was my second and more useful bot. The idea for the bot came from [Aadi Bajpai](https://aadibajpai.com/) - a dev friend.

Discord doesn't print out GitHub snippets like other chat services like Slack. This is understandably quite annoying for devs on Discord and is a very-easy-to-solve but somehow unaddressed problem.

This bot was also quite fun and easy to make - I just had to use regex and query from the GitHub API. I also added GitLab support because why not. What wasn't fun though, was all the error handling.

I've been told before that I should never trust raw user input. I had never found raw user input to be a problem until I made this bot. Within 2 hours of being added to one server, someone broke the bot by causing a runtime error.

Here's a few of the errors I had to deal with:

- Someone tried to print a 1000-line long snippet
  - This not only exceeded Discord's character limit but also made my bot slow for a while
- Someone tried to reference lines that didn't exist
  - This caused indexing errors and was what originally crashed my bot
- Someone tried to inject Markdown into the snippet to mention @everyone
- Someone tried to reference a blank line
  - This caused ugly and unexpected output, like the "code" just showing the file extension

However, fixing all these problems was worth it. Git the lines currently has 10 stars on GitHub and over 2500 users. I also learned some cool stuff from making this bot, such as the `aiohttp` library, how `asyncio` works, and how to use the GitHub API.
