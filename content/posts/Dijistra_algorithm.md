+++ 
draft = false
date = 2019-07-09T00:06:06+07:00
title = "Dijikstra Algorithm"
description = ""
slug = "" 
tags = []
categories = []
externalLink = ""
series = []
+++
# Dijikstra Algorithm
08/07/2019

## I. Mục đích:
Tìm đường đi ngắn nhất từ **1 node** (starting node) đến **tất cả** các nodes còn lại.  
**Nhược điểm**: Kết quả **SAI** khi đồ thị **cạnh âm** chứ chưa cần có **mạch âm**.  
**Ưu điểm**: So với Bellman-ford, Dijistra hiệu quả hơn vì chỉ duyệt qua các cạnh đúng 1 lần.  

## II. Ý tưởng:
Luôn lan truyền đến **node gần nhất** với **starting node x**.  
***Vì sao nó đúng?***  

>Vì Dijistra luôn vươn đến đỉnh gần gốc nhất (gốc là starting node). Điều đó có nghĩa là những đỉnh mà nó vươn tới sau sẽ có đường đi xa hơn. Vậy thôi, thì ở 1 trạng thái xyz nào đó nó vươn tới đỉnh i thì còn mong chờ gì nữa là lúc sau nó sẽ đến i bằng con đường ngắn hơn?? **No way**.  

Điều đó mang đến cho ta nhận xét:  

- *whenever a node is selected, its distance is final.*

## III. Implement
Lưu đồ thị ở dạng ***adjacent list***  
$adj[a]$ chứa một $pair(b, w)$ (cạnh $a-b$ có trọng số là $w$).  

Sử dụng `priority_queue` để select được đỉnh gần gốc nhất trong thời gian logaric. 

Những thằng gần gốc nhất mà chưa được chọn thì chỉ có thể là những thằng mà ở trạng thái đó nó có thể đi tới thôi.  

```cpp
for (int i = 1; i <= n; i++)
    distance[i] = INF;
distance[x] = 0;
q.push({0,x});
while(!q.empty()){
    int a = q.top().second; q.pop();
    if (processed[a]) continue; //bởi vì cùng dis[a] có thể push nhiều lần và priority_queue thì ko thể remove nên vậy đó.
    processed[a] = true; //có thể có nhiều đỉnh trong pqueue vì do cứ nhỏ hơn là nó push vào thôi mà
    for (auto u : adj[a]){
        int b = u.first, w = u.second;
        if (distance[a]+w < distance[b]){
            distance[b] = distance[a] + w;
            q.push({-distance[b], b});
        }
    }
}
```
$$ \Rightarrow O(n + m\log(m))$$
Chú ý một vài trick ở cài đặt:  

- Dùng $-d$ để tận dụng `default priority_queue` q
- Vài nét về `priority_queue`:
    + `priority_queue<int> pq;` //mặc định sắp xếp tăng và top() queue sẽ là $max$. 
    + `priority_queue<int, vector<int>, greater<int>> pq;`` //sắp xếp giảm và top() queue là $min$.
    + Có thể dùng `multiset<int> mset;` cùng 2 thao tác `insert`, `remove` như 1 `min_priority_queue` (as multiset default)

- Nhận xét bên lề: Mặc định của `priority_queue` là phần tử ở `top()` là `max`:
    + Elements are popped from the *back* of the specific container, which is known as the top of the priority_queue. $(top \rightarrow back)$
    + Hàm $cmp$ mặc định là $less$, nghĩa là 2 phần tử thỏa thì nó sẽ không swap, để ý là less là mặc định sort tăng đó, khi sort tăng thì thằng `back` (trong priority_queue là `top`) luôn là thằng lớn nhất rồi.
