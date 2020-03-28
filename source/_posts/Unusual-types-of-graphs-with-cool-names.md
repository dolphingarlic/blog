---
title: Unusual types of graphs with cool names
date: 2020-03-28 21:16:56
tags:
    - Programming
    - Random
    - Graphs
---

Recently, I've come across some unusual types of graphs while solving problems. These graphs often had funny and/or cool names that I thought described what they look like quite well.

Today, I'll share some of these graphs with you (along with some illustrations). If you feel like there are some other cool graphs I should add to this list, please comment them down below.

## Onion

An onion is a graph where there are exactly 2 nodes with degree greater than 2. Every other node has degree 2. Every simple path between the 2 "endpoints" has equal length.

{% asset_img onion.jpg Onion Graph %}

Onions are one of my favourite vegetables in real life, so naturally I quite like them. They're also quite easo to work with (since one can just find the two "endpoints" and DFS).

The first (and only) time I used onions was in the 2016 Polish Olympiad in Informatics problem [_Amusing Journeys_](https://szkopul.edu.pl/problemset/problem/YY6-3ua-C1rt7q-97laWc0UP/site/?key=statement).

## Cactus

A cactus is a graph where each edge is part of at most 1 simple cycle.

{% asset_img cactus.jpg Cactus Graph %}

Personally, I find cacti quite nasty to work with. They're basically trees but with annoying cycles you have to deal with (just like how cacti are basically trees but toxic and painful to touch in real life). Some people (like Brian Chau) seem to love them for some inexplicable reason though.

A nice problem that uses cacti is the 2016 Polish Olympiad in Informatics problem [_Hydrocontest_](https://szkopul.edu.pl/problemset/problem/y9HM1ctDU8V8xLMRUYACDIRs/site/).

## Caterpillar

A caterpillar is a tree where each node is either part of or exactly 1 edge away from a "central path".

{% asset_img caterpillar.png Caterpillar Tree %}
(I couldn't find a nice cartoon caterpillar so here's a Wikipedia image :P)

I don't have much to say about caterpillars. You should check out [this other blog I found](http://torus2torus.blogspot.com/2012/10/i-spy-with-my-little-eyea-caterpillar.html) if you would like to learn more about caterpillars.

A nice problem that uses them is the 2016 International Olympiad in Informatics problem [_Shortcut_](https://oj.uz/problem/view/IOI16_shortcut).
