---
title: APIO 2013 Robots
date: 2020-06-25 20:01:46
tags:
    - APIO
    - DP
categories:
    - Programming
    - Olympiad
mathjax: true
---

I'm mainly writing this post because there isn't any (good) editorial for [APIO 2013 Robots](https://oj.uz/problem/view/APIO13_robots) online. In my opinion, it's an okay problem, but it's not particularly interesting.

I'm not going to go over the problem statement because it's very long and complicated, so I'll just trust that you've already read the problem.

<!-- more -->

Firstly, notice that there are only 2 ways we can get an $(A, B)$ robot on square $(x, y)$. We can either merge an $(A, C)$ robot with a $(C + 1, B)$ robot or we can push an existing $(A, B)$ robot on $(x', y')$ several times to get to $(x, y)$.

This gives us a basic DP formulation - let $dp[x][y][A][B]$ be the minimum number of moves to get an $(A, B)$ robot on square $(x, y)$.

Initially, $dp[x_i][y_i][i][i] = 0$ for each $1 \leq i \leq N$ and $dp[x][y][A][B] = \infty$. We process the DP in increasing order of $B - A$.

First, do a DFS to find where a robot on square $(x, y)$ ends up if we push it in each of the 4 directions. This can be done in $O(HW)$ time and we only do it once - any robot pushed from the same square in the same direction will end up in the same end square.

For each pair $(A, B)$, we first handle the first way we can form an $(A, B)$ robot. Simply iterate through each square $(x, y)$ and do

$$dp[x][y][A][B] = \min_{A \leq C < B}(dp[x][y][A][C] + dp[x][y][C + 1][B])$$

This runs in $O(HWN^3)$ time.

Next, we handle the second case.

At first, I thought of using Dijkstra, but that turned out to be just a bit too slow and only got me 60 points. Then I realized that since each push costs only 1 move, I could do a modified BFS.

Notice how the maximum answer (assuming it exists) is at most $HW$. This means that instead of a priority queue, we can simply keep array of $HW$ queues acting as a big queue.

Using this, we can effectively do BFS even with varying starting costs.

For any pair $(A, B)$, this BFS runs in $O(4 \cdot HW)$ time ($O(4 \cdot HW)$ nodes/edges and $O(HW)$ queues we need to process).

In total, this solution runs in $O(N^2 \cdot (NHW + 4 \cdot HW))$ time, which is good enough to get us 100 points.

```cpp
#include <bits/stdc++.h>
#define FOR(i, x, y) for (int i = x; i < y; i++)
using namespace std;

const int INF = 250000;

int n, h, w;
char g[500][500];
pair<int, int> end_pos[500][500][4];
int dp[500][500][9][9];
vector<pair<int, int>> q[INF];

bool inside(int x, int y) {
    return (x >= 0 && y >= 0 && x < h && y < w && g[x][y] != 'x');
}

int main() {
    ios_base::sync_with_stdio(0);
    cin.tie(0);
    cin >> n >> w >> h;
    memset(dp, 0x3f, sizeof(dp));

    FOR(i, 0, h) FOR(j, 0, w) {
        cin >> g[i][j];
        if (g[i][j] - '0' > 0 && g[i][j] - '0' < 10) {
            dp[i][j][g[i][j] - '1'][g[i][j] - '1'] = 0;
        }
    }

    // Determine final positions from each push
    FOR(i, 0, h) FOR(j, 0, w) if (g[i][j] != 'x') {
        FOR(k, 0, 4) {
            pair<int, int> pos = {i, j};
            int d = (k + (g[i][j] == 'A') * 3 + (g[i][j] == 'C')) % 4;  // NESW

            while (true) {
                if (d == 0) {
                    if (inside(pos.first - 1, pos.second)) pos.first--;
                    else break;
                } else if (d == 1) {
                    if (inside(pos.first, pos.second + 1)) pos.second++;
                    else break;
                } else if (d == 2) {
                    if (inside(pos.first + 1, pos.second)) pos.first++;
                    else break;
                } else {
                    if (inside(pos.first, pos.second - 1)) pos.second--;
                    else break;
                }

                if (g[pos.first][pos.second] == 'A') d = (d + 3) % 4;
                if (g[pos.first][pos.second] == 'C') d = (d + 1) % 4;
            }

            end_pos[i][j][k] = pos;
        }
    }

    // Find the minimum no. of pushes to get robot k-l to block (i, j)
    FOR(rng, 0, n) {
        FOR(k, 0, n - rng) {
            int l = k + rng;
            FOR(i, 0, h) FOR(j, 0, w) if (g[i][j] != 'x') {
                FOR(d, k, l) {
                    dp[i][j][k][l] =
                        min(dp[i][j][k][l],
                            dp[i][j][k][d] + dp[i][j][d + 1][l]);
                }
            }
        }

        FOR(k, 0, n - rng) {
            int l = k + rng;

            FOR(i, 0, h) FOR(j, 0, w) if (g[i][j] != 'x') {
                if (dp[i][j][k][l] <= INF) q[dp[i][j][k][l]].push_back({i, j});
            }
            
            FOR(i, 0, INF) {
                for (pair<int, int> pos : q[i]) {
                    int x, y;
                    tie(x, y) = pos;
                    if (dp[x][y][k][l] == i) {
                        FOR(d, 0, 4) {
                            int nx, ny;
                            tie(nx, ny) = end_pos[x][y][d];
                            if (dp[nx][ny][k][l] > dp[x][y][k][l] + 1) {
                                q[dp[nx][ny][k][l] = dp[x][y][k][l] + 1].push_back({nx, ny});
                            }
                        }
                    }
                }
                q[i].clear();
            }
        }
    }

    int ans = INT_MAX;
    FOR(i, 0, h) FOR(j, 0, w) ans = min(ans, dp[i][j][0][n - 1]);
    cout << (ans > INF ? -1 : ans);
    return 0;
}
```
