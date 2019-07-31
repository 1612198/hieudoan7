+++ 
draft = false
date = 2019-07-31T22:01:16+07:00
title = "Articulation Point (Cut Point) & Bridge"
description = ""
slug = "" 
tags = []
categories = []
externalLink = ""
series = []
+++
# Articulation Point & Bridge
31/07/2019

## I. Definition
Ta xét trong undirected graph.  

- **Articulation Point** (đỉnh khớp): Là đỉnh mà nếu cắt bỏ đi sẽ làm **disconnect graph** (trong connected graph) còn đối với disconnected graph thì nó làm tăng số connected component.  
- **Bridge**: Là cạnh mà nếu bỏ đi ... (giống khớp)  

## II. Finding Articulation Point
### 1. Thuật toán tầm thường
Với mỗi đỉnh trong graph, ta thử cắt bỏ đi và đếm số thành phần liên thông. Nếu tăng thì đỉnh đó là khớp, ngược lại thì không. Gắn đỉnh đó lại và thử bỏ đỉnh khác cho đến khi ta thử hết các đỉnh của graph.  
$$ \Rightarrow O(V*O(DFS)) = O(V(V+E))$$

### 2. Thuật toán cải tiến (Tarjan)
Tận dụng sức mạnh của DFS.  
Nhắc lại, các loại edges trong 1 tree. 

<center>

![tree_edges](/imgs/Tree_edges.png)
</center>
Trong DFS Tree, chỉ có **Back Edges** và **Tree Edges**.  

Xét 1 đỉnh $u$ trong đồ thị:  

- Nếu $u$ là **root** và $u$ có 2 con trở lên $\Rightarrow u$ là khớp  
- Nếu $u$ khác root và **subtree rooted at v** (children of u) **has NOT** back edge to ancestor of u  $\Rightarrow u$ là khớp.  

Minh họa:  
<center>

![AP_minhhoa](/imgs/AP_minhhoa.jpg)
</center>
Nếu không có **back edge** thì khi bỏ $u$ sẽ diconnect graph với subtree rooted at v. $\Rightarrow u$ là khớp.  

### 3. Implementation
Làm thế nào để ta biết được 1 đỉnh $u$ có phải là khớp hay không?

- Đối với trường hợp $u$ là **root**: ta cứ đếm số con tại mỗi node, nếu $u$ là **root** và có $>=2$ con thì $u$ là khớp.  
- Trường hợp 2 ta lưu thêm:
  + $disc[u]$: thời gian khám phá ra đỉnh $u$ (discovery)
  + $low[u]$: $min(disc[u], disc[w])$, trong đó $w$ is ancestor of u and there is a back edge from some descendant of $u$ to $w$. Tức là ta lưu lại số thứ tự của node sớm nhất (có thể = cha $u$ nhưng theo con đường back edge của đám con cháu) mà từ đó có thể đi tới cây con rooted at $u$ (include $u$)  
  > Thì khi $low[v] \geq disc[u]$ chứng tỏ $u$ là AP ($"="$ khi back edge của con $v$ nó trỏ ngay $u$)  
  <center>

![lowv_equal_discu](/imgs/lowv_equal_discu.jpg)
</center>

$\Rightarrow $Code: Tao xin copy nguyên source trên trang [cp-algorithm](https://cp-algorithms.com/graph/cutpoints.html) bởi vì nó code quá tuyệt vời.  
$tin$ = $disc$ (time into node).  
```cpp
int n; // number of nodes
vector<vector<int>> adj; // adjacency list of graph

vector<bool> visited;
vector<int> tin, low;
int timer;

void dfs(int v, int p = -1) {
    visited[v] = true;
    tin[v] = low[v] = timer++;
    int children=0;
    for (int to : adj[v]) {
        if (to == p) continue;
        if (visited[to]) {
            low[v] = min(low[v], tin[to]);
        } else {
            dfs(to, v);
            low[v] = min(low[v], low[to]);
            if (low[to] >= tin[v] && p!=-1)
                IS_CUTPOINT(v);
            ++children;
        }
    }
    if(p == -1 && children > 1)
        IS_CUTPOINT(v);
}

void find_cutpoints() {
    timer = 0;
    visited.assign(n, false);
    tin.assign(n, -1);
    low.assign(n, -1);
    for (int i = 0; i < n; ++i) {
        if (!visited[i])
            dfs (i);
    }
}
```
## III. Finding Bridges
Hoàn toàn tương tự, chỉ khác ở điều kiện:  
<center>
$low[v] > disc[u]$ thay vì $low[v] \geq disc[u]$
</center>

### IV. Tham khảo
- competive programming 3 (Steven Halim)
- [Articulation Point GFG](https://www.geeksforgeeks.org/articulation-points-or-cut-vertices-in-a-graph/)
- [cp-algorithm](https://cp-algorithms.com/graph/cutpoints.html)