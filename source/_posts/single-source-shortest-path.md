---
title: single-source-shortest-path
date: 2023-05-23 13:29:07
tags:
- algorithm
- Dijkstra Algorithm
- Bellman-Ford Algorithm
categories:
- algorithm
- sssp
---

This post record 2 sssp (single source shortest path) algorithm.

> the following pseudo code is from this [website](https://visualgo.net/zh/sssp).

## Dijkstra Algorithm

### pseudo code

```python pseudo
show warning if the graph has -ve weight cycle
initSSSP, PQ.push((0,sourceVertex))
while !PQ.empty() // PQ is a Priority Queue
  u = PQ.front(), PQ.pop()
  for each neighbor v of u if u is valid
    relax(u, v, w(u, v)) + insert new pair to PQ
```

首先检查是否存在负距离边，因为不能处理负距离边。

然后，以 source vertex 为起点，将("已知可达最短距离", "可达 vertex 编号") 信息加入队列。之后，每次根据队列信息加入新的队列内容（更新结点可达域，获得更广的已知可达范围，或者把旧的某个距离缩小）。

队列内存储结点信息，对应结点的 neighbor 是待检查进一步优化的结点。如果可以做进一步优化，就把邻居和更新后的邻居距离加入队列。队列非空表示仍然可能继续优化。

> Prim 算法和 Dijkstra 算法都是逐渐加入 vertex 的算法，二者的不同主要在于衡量对象的不同：  
> Prim 构造MST，因此需要维护未标记点（集合）到已标记集合的距离  
> Dijkstra 需要获得到 source 的最短距离，因此需要维护未标记点（集合）到原点的距离。

### a python example                     

```Python
from heapq import heappush, heappop

def main():
    INF = int(1e9)

    # Graph in Figure 4.17
    # 5 7 0
    # 0 1 2
    # 0 2 6
    # 0 3 7
    # 1 3 3
    # 1 4 6
    # 2 4 1
    # 3 4 5

    f = open("dijkstra_in.txt", "r")

    V, E, s = map(int, f.readline().split(" "))
    AL = [[] for u in range(V)]
    for _ in range(E):
        u, v, w = map(int, f.readline().split(" "))
        AL[u].append((v, w))                     # directed graph

    # (Modified) Dijkstra's routine
    dist = [INF for u in range(V)]
    dist[s] = 0
    pq = []
    heappush(pq, (0, s))

    # sort the pairs by non-decreasing distance from s
    while (len(pq) > 0):                    # main loop
        d, u = heappop(pq)                  # shortest unvisited u
        if (d > dist[u]): continue          # a very important check
        for v, w in AL[u]:                  # all edges from u
            if (dist[u]+w >= dist[v]): continue # not improving, skip
            dist[v] = dist[u]+w             # relax operation
            heappush(pq, (dist[v], v))  

    for u in range(V):
        print("SSSP({}, {}) = {}".format(s, u, dist[u]))

main()
```
## Bellman-Ford Algorithm

与 `Dijkstra` 算法“加点”的特征相比， `Bellman-Ford` 算法最显著的特征是“加边”。

### pseudo code
```java pseudo
for i = 1 to |V|-1
  for each edge(u, v) in E // in Edge List order
    relax(u, v, w(u, v))
for each edge(u, v) in E
  if can still relax that edge, -∞ cycle found
```

n-1次循环，每次尝试更新到指定`vertex` u 的距离（每轮循环内，对全体边做一次遍历）。

若经历了 n-1 此循环后仍然可以更新，表明存在一个负距离环。

### a python example: 

```python
def main():
    INF = int(1e9)

    # Graph in Figure 4.18, has negative weight, but no negative cycle
    # 5 5 0
    # 0 1 1
    # 0 2 10
    # 1 3 2
    # 2 3 -10
    # 3 4 3

    # Graph in Figure 4.19, negative cycle exists, Bellman Ford's can detect this
    # 3 3 0
    # 0 1 1000
    # 1 2 15
    # 2 1 -42

    f = open("bellman_ford_in.txt", "r")

    # V: number of Vertexes
    # E: number of Edges
    # s: the source vertex we'll concern in Bellman-Ford Algo
    V, E, s = map(int, f.readline().split(" "))
    AL = [[] for u in range(V)]
    for _ in range(E):
        u, v, w = map(int, f.readline().split(" "))
        AL[u].append((v, w))

    # Bellman Ford's routine, basically = relax all E edges V-1 times
    dist = [INF for u in range(V)]               # INF = 1e9 here
    dist[s] = 0
    # at most V - 1 times of loop
    for i in range(0, V-1):                      # total O(V*E)
        modified = False                         # optimization
        for u in range(0, V):                    # these two loops = O(E)
            if (not dist[u] == INF):             # important check
                for v, w in AL[u]:
                    if (dist[u]+w >= dist[v]): continue # not improving, skip
                    dist[v] = dist[u]+w          # relax operation
                    modified = True              # optimization
        if (not modified): break                 # optimization

    hasNegativeCycle = False
    for u in range(0, V):                        # one more pass to check
        if (not dist[u] == INF):
            for v, w in AL[u]:
                if (dist[v] > dist[u] + w):      # should be false
                    hasNegativeCycle = True      # if true => -ve cycle
    print("Negative Cycle Exist? {}".format("Yes" if hasNegativeCycle else "No"))

    if (not hasNegativeCycle):
        for u in range(0, V):
            print("SSSP({}, {}) = {}".format(s, u, dist[u]))

main()
```