+++ 
draft = "false"
date = 2019-06-18T15:01:41+07:00
title = "Kosaraju's Algorithm Intuition"
description = ""
slug = "" 
tags = []
categories = []
externalLink = ""
series = []
+++

# Kosaraju 's Algorithms
17/06/2019

## I. DFS Tree 
Output của thuật toán DFS là 1 cây khung (spanning tree)
Tất cả các cạnh của đồ thị gốc sẽ được chia làm 4 loại trong DFS spanning Tree:
+ Tree Edge  (sometimes Tree Edge được xếp vào Forward Edge)
+ Forward Edge 
+ Back Edge
+ Cross Edge

<p align="center">
<img src="/imgs/Tree_edges.png">
</p>

**Example**
<p align="center">
<img src="/imgs/example.jpg">
</p>

If the original graph is undirected then all of its edges are **tree edge** or **black edge**

[More information about DFS, BFS Tree](https://www8.cs.umu.se/kurser/TDBA77/VT06/algorithms/LEC/LECTUR16/NODE15.HTM)

## II. Kosaraju 's Algorithm
### 1. Algorithms
* Step 1: Store finish time order of node in Stack S following DFS Traversal.
* Step 2: Transpose Graph (reverse all edges)
* DFS following nodes in the stack S

### 2. How does it work?
Ở step 1, nó tạo ra 1 cái order rất đặc biệt có tính chất: A cross edge is never from a lower ordered node to a higher ordered node. (order~num)

[The best video explain ituition Kosaraju's algorithm](https://www.youtube.com/watch?v=RpgcYiky7uw)

**Intuition Explaination:**
<p align="center">
<img src="/imgs/intuition explanation.jpg">
</p>

Goodbye, any comment is appreciated for me.

