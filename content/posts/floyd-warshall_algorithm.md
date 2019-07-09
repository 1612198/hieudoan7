+++ 
draft = false
date = 2019-07-09T21:06:45+07:00
title = "Floyd-Warshall Algorithm"
description = ""
slug = "" 
tags = []
categories = []
externalLink = ""
series = []
+++
# Floyd-Warshall algorithm
09/07/2019

## I. Mục đích
**All pair shortest path**  
Tìm đường đi ngắn nhất giữa **tất cả các cặp đỉnh**.  

## II. Ý tưởng:
Quy hoạch động dựa vào đỉnh trung gian.  

Với đỉnh trung gian đang xét là $k$, thì những $shortest\\_path$ mà có các đỉnh trung gian thuộc $[1..k]$ sẽ được tìm thấy (kết quả cuối cùng luôn), còn những cái path kia thì có ngắn hơn nhưng chưa phải ngắn nhất vì nó còn đường ngắn hơn là đi qua đỉnh trung gian mà ta chưa cover tới. Cho nên khi ta cover hết tất cả các đỉnh thì đảm bảo mọi $shortest\\_path$ giữa các đỉnh luôn được tìm thấy.  
(có chút gì đó nó khá tương đồng với Ford-Bellman)  
$$shortest\\_path (i,j,k) = min \begin{cases}shortest\\_path(i,j,k-1)\\\shortest\\_path(i,k,k-1) + shortest\\_path(k,j,k-1) \end{cases}$$  
với $shortest\\_path (i,j,k)$ là $shortest\\_path$ từ đỉnh $i$ đến $j$ khi ta đã cover $k$ đỉnh trung gian. Ghi công thức rõ ràng vậy thôi, chứ $distance[i][k]$ thì không thể nào có $k$ làm trung gian được nên là $k-1$ đỉnh trung gian đó.    

## II. Implement
Đầu tiên, khởi tạo các giá trị $distance$.  
$distance[i][i]=0$, còn có những cạnh $(u-v)$ nào thì $distance[u][v] = distance[v][u] = w(u-v)$    
```cpp
for (int i=1; i<=n; i++){
    for (int j=1; j<=n; j++){
        if (i == j) distance[i][j] = 0; //đối xứng qua đường chéo chính full 0 đó.
        else if (adj[i][j]) distance[i][j] = adj[i][j];
        else distance[i][j] = INF;
    }
}
```
After this, shortest path distances can be found as follows:  
```cpp
for (int k=1; k<=n; k++){
    for (int i=1; i<=n; i++){
        for (int j=1; j<=n; j++){
            distance[i][j] = min(distance[i][j], distance[i][k] + distance[k][j]);
        }
    }
}
```
$$\Rightarrow O(n^3) $$

**Tham khảo**: book_Antti Laaksonen.pdf  

