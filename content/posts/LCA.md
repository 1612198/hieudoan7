+++ 
draft = false
date = 2019-12-17T14:48:39+07:00
title = "Lowest Common Ancestor (LCA)"
description = ""
slug = "" 
tags = []
categories = []
externalLink = ""
series = []
+++
# Lowest Common Ancestor (LCA)
17/12/2019

## I. Bài toán
Trả lời nhiều queries của bài toán tìm LCA 2 nodes u, v trong 1 rooted **tree**.  
Lưu ý u, v phải nằm trong 1 cái **tree**, điều đó đảm bảo every nodes have only 1 parent (aka $2^0$-th parent).  

## II. Ý tưởng
Có nhiều cách tìm LCA, mình sẽ trình bày cách phổ biến nhất có tên là **binary raise** (or binary lifting).  

Ý tưởng chính:  
    + $\ $ Ta dùng ý tưởng quy hoạch động để build được  
    $\ \ \ \ \ $  $p[i][j]$: node cha thứ $2^j$ của node $i$.  
    + $\ $ Mọi số tự nhiên đều có thể phân tích thành tổng các lũy thừa của $2$. Đơn giản đó chính là binary representation của số đó thôi. (cơ bản là do chỉ có 2 hệ số 0, 1).  
$\rightarrow$ Mỗi query tốn $O(log\ n)$, precompute tốn $O(n\ log\ n)$.  
## II. Implementation
```cpp
const int N = 1e5+5;
const int LG = 20; 
int n, tree[N];
int Lv[N], p[N][LG];
void dfs(int v){  //assign Lv[root] = 1 before call dfs(root)
    for(auto u:g[v]){
        if (Lv[u]) continue;
        p[u][0] = v; //important!
        Lv[u] = Lv[v] + 1;
        dfs(u);
    }
}
void precompute(){
    for(int j=1; j<LG; j++){
        for(int i=1; i<=n; i++) {
            p[i][j] = p[p[i][j-1]][j-1]; //just typical DP technique
        }
    }
}
int LCA(int u, int v){
    if (Lv[u] < Lv[v]) swap(u, v);
    int diff = Lv[u] - Lv[v];
    for(int i=LG-1; i>=0; i--){
        if ((1<<i)&diff) { //from u, we jump up (later step shorter than sooner step)
            u = p[u][i];
        }
    }
    //after that, Lv[u] == Lv[v]
    if (u == v) return u;
    for(int i=LG-1; i>=0; i--){
        if (p[u][i] != p[v][i]) { //from lca(u, v) go up, it's all the same
            u = p[u][i];
            v = p[v][i];
        }
    }
    //now: u, v locate is the child of lca(u, v);
    return p[u][0]; //=p[v][0]
}
int main() //use
{
    Lv[1] = 1; //don't miss it
    dfs(1);  //take 1 to be root of the tree.
    precompute();
    while(queries--){
        int lca = LCA(u, v);
    }
}
```
Bên trên là dạng cổ điển tìm LCA, có nhiều bài toán vận dụng ý tưởng đó, ta có thể tính theo cạnh lớn nhất ở path từ u->v, cái ý tưởng 'binary raise' có thể giúp ta carry cái mình cần trong 1 matrix $O(n\ log\ n)$ như $p[N][LG]$

Ngoài ra, mình còn thấy có kiểu jump theo step khá tổng quát như sau:
```cpp
int jump(int v, int step){
    for(int i=LG-1; i>=0; i--){
        if ((1<<i)&step) {
            v = p[v][i];
        }
    }
    return v;
}
```  
Và timein, timeout come to rescue để xác định ADN của thằng u có phải là hậu bối của v không như sau.
```cpp
bool isAncestor(int u, int v){
    return tin[u] > tin[v] && tout[u] < tout[v];
}
// I prefer set tin and tout like that
int timer = 0;
void dfs(int v){
    tin = timer++;
    //do stuff
    tout = timer++;
}
```

## III. Problems
[Codeforces LCA Problem Collection](https://codeforces.com/blog/entry/43917)