---
layout: post
title: "Alg-Shortest-Path-with-DP"
markdown: kramdown
date: 2020-05-03
---

$ G = (V,E) $ 
$V$ = set of vertices $n = |V|$  
$E$ = set of edges $m = |E|$ $E \in V \times V without {(v,v)}$ don't have loops  
Directed Graph  $\overrightarrow{G} = (V, \overrightarrow{E})$  
Undirected Graph  

Given: $\overrightarrow{G} = (V, \overightarrow{E})$  
Goal: The shortest distance from vertex $s$ to vertex $v$.  
Def: Distance between two vertices as $d(s,v) = dist(s,v) = min_{P} \{ \sum_{e \in P} w(e) \}$, $P = {s \rightarrow v_1 \rightarrow ... \rightarrow v}$  

* Dijkstra's algorithm  
Find: distance from a vertex $s$ to any other vertex $v$ on $\overrightarrow{G}$ directed graph with $w(w) \geq 0$.  
Running time $O(|V| + |E| .log(|V|))$  

* DP shortest path  
Input: $\overrightarrow{G} = (V, \overrightarrow{E})$ w: $\overrightarrow{E} \rightarrow R$ 
$s \in V$ source  
Output: $dist(s,v)$ for all $v \in   
Step1:   
$D(i,v)$ is the shortest distance from $s$ to $v$ visiting at most $i$ vertices after $s$ and before $v$, $v \in V, 0\leq i \leq n-2$  
Step2:   
$D(i,s) = 0$, for all $ 0 \leq i \leq n-2$  
$$
D(0,v) = 
\begin{cases}
w(sv), if sv \in E \\
\inf else
\end{cases}
$$  
D(i+1, v)  = min_{y,s.t. (yv) is an edge} {D(i,y)+w(yv), D(i,v)}  
Step3(Bellman-ford):  
For $w \in V$  
    $$
    D(0,w) = 
    \begin{cases}
    0, if w = s\\
    \inf, else
    \end{cases}
    $$  
For $i = 1 to n-1$:  
    for each edge(u,v) with weight w in edges:
        D(i,v) = D(i-1,v)
        if $D(i,v) > D(i-1,u) + w(uv)$  
            D(i,v) = D(i-1,u) + w(uv)  
For each edge(u,v) with weight w in edges:
    if D(i,u) + w < D(i,v):
        error"there exists negative cycles"
return D(n-1,v) ( $v \in V$ )  
Step4:  
Running time: $O(|V||E|)$  

We can detect negative cycle using Bellman-ford algorithm by running one more iteration.  

* Floyd-warshall  
In order to compute the shortest path <em>between all the pairs</em>, construct a bigger table.  
Step1:  
$D(i,s,v)$ is the dist(s,v) along a path visiting a subset of the first $i$ vertices  
Set an ordering: $v_1, v_2, v_3, ..., v_n and v$.  
Step2:  
$$
D(0,s,v) = 
\begin{cases}
w(sv), if sv \in E \\
\inf, else
\end{cases}
$$  
$ D(0, s, s) = 0 $  
$ D(i,s,v) = min{ D(i-1, s,v_i) + D(i-1,v_i,v), D(i-1,s,v)}$  
Step3:  
for i = 1 to n:  
    for j = 1 to n:  
        dist(i, j, 0) = ∞
for all $(i, j) \in E$:  
    dist(i, j, 0) = `(i, j)  
for k = 1 to n:  
    for i = 1 to n:  
        for j = 1 to n:  
            dist(i, j, k) = min{dist(i, k, k − 1) + dist(k, j, k − 1), dist(i, j, k − 1)}  
Step4:  
Running time: $O(|V|^3)$





