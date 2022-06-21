---
title: Quantum Mechanics But With Socks
date: 2022-06-20 22:12:00
tags:
  - Physics
  - Quantum Mechanics
categories:
  - Materials Science
  - 3.033
mathjax: true
---

I recently learnt why Heisenberg's uncertainty principle is the way it is, so I thought it would be cool to explain three concepts in quantum mechanics in a fun way. Disclaimer: I'm not an expert in quantum mechanics, and I'm still learning it myself. If the following explanations are not technically correct, please let me know in the comments below.

<!-- more -->

# Superposition

Imagine you have a single sock. It's unmarked and could fit on either one of your feet. This sock is in a **superposition** of left and right. It could be in either **state**, but we don't know which one until we take a **measurement** (i.e. put on the sock). Note that the sock isn't exactly in both states at once, but rather has a **probability** of being in either state.

Mathematically, we can express the state of the sock before measurement as the two possible measured states **superposed** (i.e. added together):

$$\lvert 🧦 \rangle = \frac{1}{\sqrt{2}} \lvert ⬅️ \rangle + \frac{1}{\sqrt{2}} \lvert ➡️ \rangle$$

The probability of getting a particular state is equal to the number in front of each measured state squared, which is 50% for both states here.

The probabilities don't always have to be equal. For example, imagine the sock has a small "L" printed on it. 50% of the time, you'd notice the letter and put the sock on your left foot, but the other 50% of the time, you'd miss it. In this case, the state of the sock would be like so:

$$\lvert 🧦 \rangle = \frac{\sqrt{3}}{2} \lvert ⬅️ \rangle + \frac{1}{2} \lvert ➡️ \rangle$$

That is, 75% of the time it's a left sock, while 25% of the time it's a right sock.

After the measurement, the superposition **collapses** and you'd be left with just one possible state. If you'd measure the state again (by looking down at your feet), you'd get the same result each time.

# Entanglement

Now imagine you have two socks that form a pair. These socks are **entangled**. If you know that one of them is the left sock, then you know the other one must be the right sock, no matter where in the universe it is. This is also **instantaneous**, no matter how far away the other sock is. Even if it's in the Andromeda galaxy, you don't have to wait 2.537 million years for it to become the right sock.

Although this seems like the perfect way to communicate over vast distances, it's actually impossible to send information using entanglement. Although measuring one sock instantly changes the state of the other sock, it's impossible for that change to be detected. Entanglement isn't a magic connection between two objects!

# Heisenberg's Uncertainty Principle

Now imagine you want to measure two more quantities -- the sock's **warmth** and **stretchiness**. Since measuring warmth and footedness can both be done while the sock is in the same kinds of states (i.e. on your foot), you can measure them simultaneously and know both with infinite precision. It also doesn't matter which one you measure first -- you get the same results either way.

However, stretchiness and warmth can't be measured at the same time, since the sock must be in different states for each (off and on your foot respectively). Measuring one quantity would **perturb** the system and change future measurements of the other (you have to take the sock off to measure its stretchiness, which changes the warmth). Because of this, we can't know both quantities at the same time with infinite precision. The order that you measure these quantities also matters here.

In quantum mechanics, this is known as **Heisenberg's uncertainty principle**. All classical observables (momentum, energy, etc.) are measured with **Hermitian operators** (operators are to functions as matrices are to vectors). If two operators **don't** commute (order matters), then we **can't** measure both of their observables simultaneously with infinite precision.

In general, $\Delta A \Delta B \geq \frac{1}{2} \left| [\hat A, \hat B] \right|$. $[\hat A, \hat B]$ is the **commutator** of observables $\hat A$ and $\hat B$, which basically measures how much they don't commute. In the specific case of momentum and position, we get $\Delta p \Delta x \geq \frac{1}{2}\left|\left[\hat p, \hat x\right]\right| = \frac{\hbar}{2}$. Wikipedia has a great mathematical proof of the inequality [here](https://en.wikipedia.org/wiki/Uncertainty_principle#Robertson.E2.80.93Schr.C3.B6dinger_uncertainty_relations) if you're interested.
