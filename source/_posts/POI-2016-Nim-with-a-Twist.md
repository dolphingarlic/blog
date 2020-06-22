---
title: POI 2016 Nim with a Twist
date: 2020-06-22 17:33:51
tags:
    - POI
    - Maths
    - DP
    - Game Theory
categories:
    - Programming
    - Olympiad
mathjax: true
---

I've always found game theory (especially Nim games) fascinating. That's why I'd like to share a beautiful game theory problem from the 2016 Polish Olympiad in Informatics with you today.

You can find the problem [here](https://szkopul.edu.pl/problemset/problem/X6IwPa2H9FSd3Ly6bYp5t8Vu/site/?key=statement) (the statement is pretty short).

The gist is that you're given the initial configuration of a classical Nim game with $N \leq 5 \cdot 10^5$ piles and you want to count the number of subsets of piles you can remove so that player 2 can guarantee a win and the number of piles you remove is divisible by $d \leq 10$.

You're also given that $a_i \leq 10^6$ ($a_i$ is the number of stones in pile $i$) and $\sum a_i \leq 10^7$.

To make it even more challenging, the memory limit is just 64mb.

<!-- more -->

We use DP to solve this problem. We know that player 2 can win if and only if the xor of the remaining piles is $0$.

## Basic DP approach

Let $dp[i][j][mask] =$ Number of subsets we can take away from the first $i$ piles such that we take away $j$ piles and the xor of the remaining subsets is $mask$.

The recurrence is $dp[i][j][mask] = dp[i - 1][j][mask] + dp[i - 1][j - 1][mask \oplus a_i]$

Clearly, the answer is $\sum_{i = 1}^{N / d}dp[N][d \cdot i][0]$.

However, computing this dp naively takes $O(N^2 \cdot \max(A_i))$ time and memory, which is far too slow to get 100 points.

## Taking advantage of the small $d$

Firstly, we can reduce the memory used significantly by getting rid of the first dimension stored.

Notice how $dp[i][j]$ depends only on $dp[i - 1][j]$ and $dp[i - 1][j - 1]$. This means we can drop the first dimension entirely and compute $dp[i]$ in-place!

Next, notice that we really only care about the remainder of $j$ modulo $d$, so we can reduce the memory and time complexities further.

Now we can calculate $dp[j % d][mask]$ in $O(d \cdot \max(a_i)^2)$ time and $O(d \cdot \max(a_i))$ memory. This fits in the memory limits, but is unfortunately still too slow.

## Taking advantage of the (relatively) small $\sum a_i$

The next big observation is that the order of the piles doesn't matter. Since the order doesn't matter, we may as well sort them.

Notice that if we process the piles in increasing order, we only have to consider $mask$ in the dp if $mask \leq 2 \cdot a_i$.

This means we can drop out time complexity down to $O(d \cdot \sum a_i)$!

This clever solution gets us 100 points and is quite neat as well.

## Code

```cpp
#include <bits/stdc++.h>
#define FOR(i, x, y) for (int i = x; i < y; i++)
typedef long long ll;
using namespace std;

const int MOD = 1e9 + 7;

int a[500001], dp[10][1 << 20], tmp[1 << 20];

void add(int &x, int y) {
    x += y;
    if (x >= MOD) x -= MOD;
}

int main() {
    ios_base::sync_with_stdio(0);
    cin.tie(0);
    int n, d, xor_all = 0;
    cin >> n >> d;
    FOR(i, 1, n + 1) {
        cin >> a[i];
        xor_all ^= a[i];
    }
    sort(a + 1, a + n + 1);

    dp[0][0] = 1;
    int curr_mx = 1;
    FOR(i, 1, n + 1) {
        while (curr_mx <= a[i]) curr_mx <<= 1;
        memcpy(tmp, dp[d - 1], sizeof(int) * curr_mx);
        for (int j = d - 1; j; j--)
            FOR(k, 0, curr_mx) add(dp[j][k], dp[j - 1][k ^ a[i]]);
        FOR(k, 0, curr_mx) add(dp[0][k], tmp[k ^ a[i]]);
    }

    cout << (dp[0][xor_all] - (n % d == 0) + MOD) % MOD;
    return 0;
}
```
