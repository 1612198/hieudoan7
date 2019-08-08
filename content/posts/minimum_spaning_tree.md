+++ 
draft = false
date = 2019-08-05T20:25:45+07:00
title = "Minimum Spanning Tree"
description = ""
slug = "" 
tags = []
categories = []
externalLink = ""
series = []
+++
# Minimum Spanning Tree  
(Cây khung nhỏ nhất)  
05/08/2019

## I. Definition
- ***Tree***: (Cây) là đồ thị vô hướng (undirected), liên thông (connected) và không có chu trình (acyclic).  
Các tính chất của cây:
    + Cây **n nodes** luôn có **n-1 cạnh**
    + Mỗi cạnh đều là **cầu**
    + Thêm 1 cạnh bất kì sẽ tạo nên chu trình.
- ***Spanning Tree:*** (Cây khung) là **Cây** chứa tất cả các nodes, so nó luôn có n-1 cạnh.  
    >  Có thể tìm cây khung sử dụng bfs, dfs.
- ***Minimum Spanning Tree:*** Là cây khung có chi phí nhỏ nhất (tổng weight min), dĩ nhiên nó cũng phải có n-1 cạnh thôi.
- Maximum Spanning Tree có thể hoàn toàn sử dụng thuật toán minimum spanning tree bằng cách đổi dấu các cạnh.  

## II. Kruscal's Algorithm
### 1. Ý tưởng:
Sắp xếp tất cả các cạnh theo thứ tự tăng của weight.  
Lần lượt lấy các cạnh để cấu thành cây khung, nếu cạnh nào làm xuất hiện cycle thì bỏ qua đi tiếp đến khi đủ $n-1$ cạnh là được. (đủ $n-1$ cạnh có nghĩa là đã cover hết các nodes).  

### 2. Why does this work?

*Trực quan:*  
Cho đồ thị có $n$ nodes và có cây khung nhỏ nhất là $e\_1, e\_2, ..., e\_{n-1}$.  

Giả sử có 1 cây khung $T$ khác mà không chứa 1 cạnh $e_i$ nào đó $(1\leq i \leq n-1)$ ở trên thì rõ ràng $T + e_i$ tạo ra cycle, từ cycle đó ta luôn có thể bỏ đi 1 cạnh $>$ $e_i$ và được cây khung nhỏ hơn. Nếu được loại 1 cạnh thì đương nhiên ta loại cạnh lớn nhất.  

More Provement in [cp-algorithms](https://cp-algorithms.com/graph/mst_kruskal.html)

### 3. Implementation
Ý tưởng cài đặt:  
Ban đầu tất cả các đỉnh rời nhau. Lấy các cạnh theo thứ tự đã sort để giảm số CC (ban đầu có n CCs). Đến khi còn 1 CC thì dừng lại.

#### Cài đặt tầm thường: 
$$\Rightarrow O(M\ log\ N + N^2)$$
($M\ log\ N \approx M\ log\ M$)

```cpp
struct Edge {
    int u, v, weight;
    bool operator<(Edge const& other) {
        return weight < other.weight;
    }
};

int n;
vector<Edge> edges;

int cost = 0;
vector<int> tree_id(n);
vector<Edge> result;
for (int i = 0; i < n; i++)
    tree_id[i] = i;

sort(edges.begin(), edges.end());

for (Edge e : edges) {
    if (tree_id[e.u] != tree_id[e.v]) {
        cost += e.weight;
        result.push_back(e);

        int old_id = tree_id[e.u], new_id = tree_id[e.v];
        for (int i = 0; i < n; i++) {
            if (tree_id[i] == old_id)
                tree_id[i] = new_id;
        }
    }
}
```
### Cài đặt dùng DSU (Disjoint Set Union) 
$$\Rightarrow O(M\ log\ N + N + M) = O(M\ log\ N)$$

```cpp
vector<int> parent, rank; 
//parent: leader (representative)
//rank: height of tree
void make_set(int v) {
    parent[v] = v;
    rank[v] = 0;
}

int find_set(int v) {
    if (v == parent[v])
        return v;
    return parent[v] = find_set(parent[v]);
}

void union_sets(int a, int b) {
    a = find_set(a);
    b = find_set(b);
    if (a != b) {
        if (rank[a] < rank[b])
            swap(a, b);
        parent[b] = a;
        if (rank[a] == rank[b])
            rank[a]++;
    }
}

struct Edge {
    int u, v, weight;
    bool operator<(Edge const& other) {
        return weight < other.weight;
    }
};

int n;
vector<Edge> edges;

int cost = 0;
vector<Edge> result;
parent.resize(n);
rank.resize(n);
for (int i = 0; i < n; i++)
    make_set(i);

sort(edges.begin(), edges.end());

for (Edge e : edges) {
    if (find_set(e.u) != find_set(e.v)) {
        cost += e.weight;
        result.push_back(e);
        union_sets(e.u, e.v);
    }
}
```
Có thể dùng $size[]$ thay cho $rank[]$, more information: [cp-algorithms](https://cp-algorithms.com/data_structures/disjoint_set_union.html)