---
title: Burnside's Lemma and Pizza
date: 2020-04-30 10:06:24
tags:
    - Maths
    - Group theory
mathjax: true
---

This post is basically a transcription of my YouTube video explaining Burnside's lemma. Please consider watching and liking that video as well.

{% youtube AC8nYkHh1vE %}

<!-- more -->

## A Pizza Problem

Imagine we're making a pizza with 8 slices. If each slice must have exactly 1 of our 3 favourite toppings and 2 pizzas are considered the same if we can rotate one to get the other, how many distinct pizzas can we make?

{% asset_img equal.png %}

If we got rid of the rotation restriction, then the answer would simply be $3^8 = 6561$, since each slice is independent and has 3 possibilities. Clearly, the answer to the original problem is much less than 6561.

One way to solve this is simply to draw out all 6561 possible pizzas and count the distinct ones, but that's slow and boring. Let's turn to a more mathematical approach.

But first, some definitions. Let $X$ be the set of all 6561 possible pizzas and $G$ be the group of the 8 ways we can rotate the pizza (rotate by 0 slices, rotate by 1 slice, rotate by 2 slices, etc.)

## Orbit

For some pizza, consider the set of pizzas we can obtain from rotating it a number of slices. This set is called an orbit.

{% asset_img orbit.png %}

The orbit of pizza $x$ is written as $O_x$ and the orbits in the set of all possible pizzas is written as $X / G$.

Notice how the number of distinct pizzas we can make is simply the number of orbits in the set of all possible pizzas.

## Fixed Point

A fixed point of a rotation is a pizza that still looks the same when rotated that amount.

{% asset_img fixed-point.png %}

The set of fixed points of a rotation $g$ is written as $X^g$.

For instance, a fixed point of a rotation by 2 slices must have identical 1st, 3rd, 5th and 7th slices, as well as identical 2nd, 4th, 6th, and 8th slices.

This means there are $3^2 = 9$ fixed points of a rotation by 2 slices.

## Burnside's Lemma

Burnside's lemma (aka Not-Burnside's lemma) simply states that the number of orbits is equal to the average number of fixed points. It's represented nicely by this formula:

$$|X / G| = \frac{1}{|G|} \sum_{g \in G} |X^g|$$

Applying Burnside's lemma to our problem, we find that there are 834 distinct pizzas we can make. It's like magic!

{% asset_img result.png %}

## Why This Works

Let's call rotations of a pizza that produce the same pizza "bad rotations."

{% asset_img bad-rotation.png %}

The set of all bad rotations of a pizza is called its stabilizer and is written as $G_x$ for pizza $x$.

Notice how the total number of fixed points is equal to the total number of bad rotations since they're essentially the same thing.

{% asset_img equiv.png %}

Now imagine for some pizza, we have a big pizza. The slices of the big pizza represent the small pizza rotated by 0 slices, then 1 slice, then 2 slices, and so on.

{% asset_img big-pizza-1.png %}

Notice how if we separate the big pizza into sectors where no two slices are the same, we get the orbits of the small pizza! (Since the big pizza is periodic).

{% asset_img big-pizza-2.png %}

The number of sectors we get is equal to the size of the stabilizer of the small pizza, so the product of the stabilizer size and orbit size of any pizza is equal to the size of the group of rotations.

$$|G_x| \cdot |O_x| = |G|$$

This result is known as the orbit-stabilizer theorem.

After plugging everything into the equation, we find that the average number of fixed points is equal to the sum of 1 over the size of each pizza's orbit.

$$\begin{align\*}\frac{1}{|G|} \sum_{g \in G}|X^g| &= \frac{1}{|G|} \sum_{x \in X}|G_x|\\\\&= \frac{1}{|G|} \sum_{x \in X}\frac{|G|}{|O_x|}\\\\&= \sum_{x \in X}\frac{1}{|O_x|}\end{align\*}$$

But what is the sum of 1 over the size of each pizza's orbit?

Going back to our big pizza analogy, each smaller pizza in an orbit contributes 1 over that orbit's size to the total sum, so the sum is therefore simply the number of orbits.

{% asset_img big-pizza-3.png %}

$$\sum_{y \in O_x}\frac{1}{|O_x|} = 1 \implies \sum_{x \in X}\frac{1}{|O_x|} = |X / G|$$

Therefore, Burnside's lemma is true.

## Conclusion

The cool thing about Burnside's lemma is that you're not limited to just rotations! Any finite group $G$ acting on a set $X$ works.

Here's a bonus problem: Instead of 8 slices and 3 toppings, find a formula for when there are $N$ slices and $M$ toppings.
