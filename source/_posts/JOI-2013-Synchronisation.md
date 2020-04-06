---
title: JOI 2013 Synchronisation
date: 2020-01-05 15:51:34
tags:
    - JOI
    - Programming
    - Olympiad
mathjax: true
---

Today, I'd like to share a really cool problem I solved from the Japanese Olympiad in Informatics. This problem is [Synchronisation](https://oj.uz/problem/view/JOI13_synchronization) from the 2013 Open Contest.

Here's a quick summary of the problem: You have $N \leq 10^5$ computers and $N - 1$ connections that form a tree. Initially, all connections are inactive and all computers hold a single unique piece of information. Each time the connection between computers $A$ and $B$ is activated, the computers connected to $A$ and $B$ (including $A$ and $B$) sync up and share information. The states of the connections are changed $M \leq 10^5$ times and you would like to know how many unique pieces of information are stored on each computer at the end.

There are many ways to solve this problem (I know of 4 distinct ways), but I'll share what I think is the most elegant solution, which relies on more observations and has relatively light implementation.

Let's solve this problem subtask by subtask, just as we would in an actual competition!

## Subtask 2

Subtask 2 is by far the easiest. Here, the network is simply a straight line.

Notice that the range of the information on each computer is a single contiguous interval. Coincidentally, so is the connected component any computer is part of. Knowing this, we can simply use a segment tree with lazy propagation to merge these ranges over an interval whenever we connect 2 computers.

The time complexity is $O(N \log N)$, which gets us 20 points.

## Subtask 1

Subtask 1 is very similar to the general problem, with the key difference being that we only have to answer 1 query.

Naturally, we'd like to root the tree at the queried node. For convenience, let this node be node 1.

Notice that for each connected component has a single node with minimum depth (kinda like the "root" of the connected subtree). This means that instead of storing information in each node of a component, we can simply store the information in that "root"!

Specifically, let $a_i$ be 1 for each node $i$. When we merge nodes $u$ and $v$ (where $u$ is the parent of $v$), $v$ is its component's "root", so we can simply apply
$$a_{root[u]} \to a_{root[u]} + a_v, a_v \to 0$$

After doing this, $a_1$ is the answer to the query. This intuitively works because we would like to "bubble" information up to node 1, which we do by shifting "fresh" information up to the root of a component. Notice how this always results in $a_v = 0$ if $v$ isn't a root.

Now how do we find the root of a component quickly?

Notice how the root $u$ of node $v$ is simply the lowest node where there are no active connections between $u$ and $v$. We can find the number of active connections between the root and a node using a DFS-order traversal and a Fenwick tree. This feels very similar to finding the LCA of two nodes, so we can simply apply binary lifting to find the root in $O(\log N)$.

The time complexity is again $O(N \log N)$, which gets us an additional 20 points.

## Subtask 3

The solution to this subtask comes naturally from that of subtask 1.

Let $c_v$ be the amount of data that was last synced between $v$ and its parent and let $a_v$ be the amount of information currently in $v$. When we connect $u$ and $v$, the new amount of data on both computers is thus $a_v + a_u - c_v$.

This means that we don't have to set $a_v$ to 0 anymore when we sync it! Now we can simply root the tree arbitrarily and apply our approach for subtask 1 - only now we also keep track of $c_v$ and don't set $a_v$ to 0.

The answer to each query $v$ is thus simply $a_{root[v]}$.

The time complexity is yet again $O(N \log N)$, which gets us 100 points.

## Code

```cpp
#include <bits/stdc++.h>
#define FOR(i, x, y) for (int i = x; i < y; i++)
typedef long long ll;
using namespace std;

int n, m, q;
bool active[100001];
vector<int> graph[100001];
pair<int, int> edges[200001];

int info[100001], last_sync[100001];

// DFS order
int timer = 1, tin[100001], tout[100001];
// Binary lifting parents
int anc[100001][20];

void dfs(int node = 1, int parent = 0) {
    anc[node][0] = parent;
    for (int i = 1; i < 20 && anc[node][i - 1]; i++) {
        anc[node][i] = anc[anc[node][i - 1]][i - 1];
    }

    info[node] = 1;

    tin[node] = timer++;
    for (int i : graph[node]) if (i != parent) dfs(i, node);
    tout[node] = timer;
}

// Fenwick tree
int bit[100001];

void update(int pos, int val) { for (; pos <= n; pos += (pos & (-pos))) bit[pos] += val; }

int query(int pos) {
    int ans = 0;
    for (; pos; pos -= (pos & (-pos))) ans += bit[pos];
    return ans;
}

// Binary lifting
int find_ancestor(int node) {
    int lca = node;
    for (int i = 19; ~i; i--) {
        if (anc[lca][i] && query(tin[anc[lca][i]]) == query(tin[node])) lca = anc[lca][i];
    }
    return lca;
}

int main() {
    ios_base::sync_with_stdio(0);
    cin.tie(0);
    cin >> n >> m >> q;
    FOR(i, 1, n) {
        cin >> edges[i].first >> edges[i].second;
        graph[edges[i].first].push_back(edges[i].second);
        graph[edges[i].second].push_back(edges[i].first);
    }
    dfs();

    FOR(i, 1, n + 1) {
        update(tin[i], -1);
        update(tout[i], 1);
    }

    while (m--) {
        int x;
        cin >> x;
        int u = edges[x].first, v = edges[x].second;
        if (anc[u][0] == v) swap(u, v);

        if (active[x]) {
            info[v] = last_sync[v] = info[find_ancestor(u)];
            update(tin[v], -1);
            update(tout[v], 1);
        } else {
            info[find_ancestor(u)] += info[v] - last_sync[v];
            update(tin[v], 1);
            update(tout[v], -1);
        }
        active[x] = !active[x];
    }

    while (q--) {
        int x;
        cin >> x;
        cout << info[find_ancestor(x)] << '\n';
    }
    return 0;
}
```
