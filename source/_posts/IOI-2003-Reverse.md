---
title: IOI 2003 Reverse
date: 2020-04-06 15:43:57
tags:
    - IOI
    - Ad-hoc
categories:
    - Programming
    - Olympiad
---

**Update: Read my explanation of my solution at https://codeforces.com/blog/entry/75726**

I finally solved [_Reverse_](https://contest.yandex.ru/ioi/contest/558/problems/C/) from IOI 2003! Here's a brief summary of what the problem is about:

You have an ancient computer that has 9 registers and 2 operations. Each register can hold an integer from 0 to 255. An `S` operation in the form `S A B` sets `register[B] = register[A] + 1`. A `P` operation in the form `P A` simply prints `register[B]`. You may choose the 9 starting values of the registers.

Your task is, for a given `N`, print out `N`, `N - 1`, ..., `0` using as few **consecutive** `S` operations as possible.

<!-- more -->

This problem was pretty nice in both concept and implementation... if you were fine with 88 points. If you wanted 100 points though, you had to be a lot more creative.

The first major hurdle was the fact that the editorial only described a solution that got 88 points. This meant that you couldn't even just copy the implementation from the editorial.

The next major hurdle was the fact that Yandex had decreased the limit of consecutive `S` operations for one test case (`N = 97`) because some IOI 2003 contestant had found a `MAX = 2` solution (the official solution was a `MAX = 3` solution). Other than that, only the `MAX = 4` cases failed for the 88-point solution.

The last major hurdle was how few people had actually solved this problem. Prior to me solving it, only 1 person ([Noam Gutterman](https://codeforces.com/profile/Noam527)) had 100 points for it on Yandex. In fact, [grandmasters on Codeforces were even doubting the existence of a 100 point solution](https://codeforces.com/blog/entry/66014?#comment-499829). Luckily though, I know Noam and was able to ask him for help.

I don't have the willpower to explain my solution, but it's a relatively simple extension of the 88-point solution. Here's my code if you want to decipher it:

```cpp
#include <bits/stdc++.h>
#define FOR(i, x, y) for (int i = x; i < y; i++)
using namespace std;

int n, k, reg[9];
int lim[5]{8, 44, 97, 116, 257};

void INIT(int V, int P) {
    printf("%d", V);
    if (P == 8) printf("\n");
    else printf(" ");
    assert(V > -1 && V < 256);
    assert(P > -1 && P < 9);
    reg[P] = V;
}

void ADD(int A, int B) {
    printf("S %d %d\n", A + 1, B + 1);
    assert(A > -1 && A < 9);
    assert(B > -1 && B < 9);
    reg[B] = reg[A] + 1;
}

int FIND(int V) { return find(reg, reg + 9, V) - reg; }

void PRINT(int P) {
    printf("P %d\n", P + 1);
    assert(P < 9);
    assert(reg[P] == n);
}

int main() {
    scanf("%d %d", &k, &n);
    printf("FILE reverse %d\n", k);

    int mx = lower_bound(lim, lim + 8, n) - lim;

    if (mx == 4 || mx == 2) {
        int c = n, diff1 = (mx == 2 ? 3 : 9), diff2 = (mx == 2 ? 2 : 0);
        INIT(c, 0);
        INIT(c - mx - 1, 1);
        c -= mx + 1;
        FOR(i, 1, 8) {
            c -= i * diff1 + diff2;
            INIT(max(c, 0), i + 1);
        }

        int nxt = 2, curr, rising = -1;
        queue<int> to_rise;
        bool twice = true;

        n++;
        while (n--) {
            curr = FIND(n);
            PRINT(curr);
            if (!n) break;

            int pos = FIND(n - 1);
            if (pos == 9) {
                FOR(i, 1, mx + 1) {
                    pos = FIND(n - 1 - i);
                    if (pos != 9) {
                        ADD(pos, curr);
                        for (int j = 1; j < i && reg[curr] < n - 1; j++) ADD(curr, curr);

                        if ((i != mx - 1 || mx == 2) && twice && rising > -1 && rising < 9 && rising != pos)
                            for (int j = i; j < mx && reg[rising] < n - 1; j++) ADD(rising, rising);

                        break;
                    }
                }
            } else {
                twice = !twice;
                if (twice && nxt != 9) {
                    if (!to_rise.size()) {
                        nxt++;
                        FOR(i, 0, nxt - 2) to_rise.push(reg[nxt] + i * diff1);
                    }
                    rising = FIND(to_rise.front());
                    to_rise.pop();
                } else {
                    FOR(j, 2, 2 * mx + 3) {
                        rising = FIND(n - j);
                        if (rising != 9) break;
                    }
                }

                if (rising != 9) {
                    ADD(rising, curr);
                    for (int i = 1; i < mx && reg[curr] < n - 2; i++) ADD(curr, curr);
                    rising = curr;
                }
            }
        }
    } else {
        int c = n;
        FOR(i, 0, 9) {
            INIT(max(0, c), i);
            c -= mx * i + mx + 1;
        }

        int nxt = 1, curr = -1, rising = 1;
        n++;
        while (n--) {
            int pos = FIND(n);
            if (pos == 9) {
                FOR(i, 1, mx + 1) {
                    pos = FIND(n - i);
                    if (pos != 9) {
                        ADD(pos, curr);
                        FOR(j, 1, i) ADD(curr, curr);
                        PRINT(curr);
                        break;
                    }
                }
            } else {
                if (pos == rising && nxt != 8) rising = ++nxt;
                if (~curr && mx && reg[rising] + mx < n) {
                    ADD(rising, curr);
                    FOR(i, 1, mx) ADD(curr, curr);
                    rising = curr;
                }
                PRINT(pos);
                curr = pos;
            }
        }
    }
    return 0;
}
```
