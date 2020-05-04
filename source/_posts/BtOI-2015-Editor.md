---
title: BtOI 2015 Editor
date: 2020-03-17 18:20:42
tags:
    - BtOI
    - Graphs
    - Binary lifting
categories:
    - Programming
    - Olympiad
mathjax: true
---

Today, I'd like to share another really cool programming problem. This problem is [Editor](https://oj.uz/problem/view/BOI15_edi) from the 2015 Baltic Olympiad in Informatics.

Here's a quick summary of the problem: You've invented a text editor where there are 2 operations: **edit** and **undo**. Each operation has a **level**. An **edit** changes the editor state (initially 0) to some new state. An **undo** of **level** $L$ undoes the most recent operation with **level** strictly less than $L$. Note that an **undo** can undo a previous **undo**. Each **edit** has **level** 0 and the user determines the **level** of each **undo**. You're now given a sequence of $N \leq 10^5$ operations. For each operation, you want to know the editor state directly after you apply it.

<!-- more -->

For example, we can have the following sequence of operations (E means **edit**; U means **undo**):

|  Operation   |     | E<sub>1</sub> | E<sub>2</sub> | E<sub>5</sub> | U<sub>1</sub> | U<sub>1</sub> | U<sub>3</sub> | E<sub>4</sub> | U<sub>2</sub> | U<sub>1</sub> | U<sub>1</sub> | E<sub>1</sub> |
| :----------: | :-: | :-----------: | :-----------: | :-----------: | :-----------: | :-----------: | :-----------: | :-----------: | :-----------: | :-----------: | :-----------: | :-----------: |
| Editor state |  0  |       1       |       2       |       5       |       2       |       1       |       2       |       4       |       2       |       1       |       0       |       1       |

The official solution is a persistent trie of some sorts, but it's long and complicated. Instead, I'll present an elegant binary lifting solution.

I'll focus on **undo** operations in this post, since nothing really happens with **edit** operations other than outputting the answer.

## Observation 1

**For 3 operations $X$, $Y$, $Z$ in order, if $Y$ can't undo $X$ and $Z$ can't undo $Y$, then $Z$ can't undo $X$.**

This observation is pretty obvious and doesn't seem very helpful at first, but it will become useful later on.

Before we move on though, view each operation as a node in a graph. If $Y$ undoes $X$, draw an edge between $Y$ and $X - 1$ and let $P_Y = X - 1$.

Notice how the answer for $Y$ is simply the answer for $P_Y$. Additionally, our graph is a forest.

![Graph from the above table](graph.png)

## Observation 2

**If $Y$ can't undo $X$, then $Y$ can't undo any operation in the range $(P_X, X]$.**

This is because $P_X + 1$ is the most recent active operation $X$ could undo. The states of the operations in the range $(P_X, X]$ also remain unchanged after applying operation $X$, so by observation 1, this is true. (This still holds when we undo some operations.)

## Observation 3

**By observation 2, the operation we undo must lie on some path from the most recent operation upwards!**

We essentially want the most recent operation with level strictly less than the current **undo**. We can use suffix minimums and binary lifting to find this operation.

The time complexity is $O(N \log N)$ and this gets us 100 points.

## Code

```cpp
#include <bits/stdc++.h>
#define FOR(i, x, y) for (int i = x; i < y; i++)
typedef long long ll;
using namespace std;

int anc[300001][20], mn[300001][20], ans[300001];

int main() {
    ios_base::sync_with_stdio(0);
    cin.tie(0);
    int n;
    cin >> n;
    FOR(i, 1, n + 1) {
        int x;
        cin >> x;
        if (x > 0) {
            ans[i] = x;
            anc[i][0] = i;
        } else {
            x = -x;
            int curr = i - 1;
            for (int j = 19; ~j; j--) if (mn[curr][j] >= x) curr = anc[curr][j];
            anc[i][0] = curr - 1;
            ans[i] = ans[curr - 1];
            mn[i][0] = x;
            FOR(j, 1, 20) {
                anc[i][j] = anc[anc[i][j - 1]][j - 1];
                mn[i][j] = min(mn[i][j - 1], mn[anc[i][j - 1]][j - 1]);
            }
        }
    }
    FOR(i, 1, n + 1) cout << ans[i] << '\n';
    return 0;
}
```
