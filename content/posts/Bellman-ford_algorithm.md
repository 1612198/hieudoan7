+++ 
draft = false
date = 2019-07-08T21:43:13+07:00
title = "Bellman-Ford Algorithm"
description = ""
slug = "" 
tags = []
categories = []
externalLink = ""
series = []
+++
# Bellman-Ford Algorithm
08/07/2019

## I. Mục đích
Tìm đường đi ngắn nhất từ **1 node** (starting node) đến **tất cả** các nodes còn lại.  

**Ưu điểm**: Có thể phát hiện **mạch âm**

## II. Ý tưởng:
Gọi $distance[i]$ là đường đi ngắn nhất từ đỉnh $x$ (starting node) đến đỉnh i ($distance[x] = 0$)  

- Sau lần `round 1`: những đỉnh $i$ kề $x$ sẽ xác định được $distance[i]$ (final value luôn đó)  
...  
- Sau lần `round thứ k`: những shortest path từ $x$ đến $i$ nào mà kết quả có $k$ cạnh thì sẽ được xác định.  

Mà shortest path từ $x$ đến $any\ i$ chỉ có thể có tối đa $n-1$ edges nên ta chỉ cần `round` $n-1$ lần là tìm được tất cả các $distance[i]$  

Về mặt hình dung ta có thể hiểu là:  
Sau `round 1` thì những đỉnh kề $i$ được **discover**, sau `round 2` thì những đỉnh kề với những `đỉnh đã discover lần trước` được **discover**, cứ vậy đến hết. Đây cũng là ý tưởng để cải tiến là ta chỉ cần duyệt các `cạnh kề với các đỉnh vừa discover` mà không cần duyệt hết `list cạnh`, tuy nhiên, cải tiến này không làm giảm độ phức **Big-Oh**.  

Nhưng vì cài đặt là ta duyệt qua edges nên may mắn nào đó nó discover hết trong 1 lần duyệt là điều có thể xảy ra.  

Nếu tất cả các $distance[i]$ đều có ít hơn $n-1$ edges thì các $distance[i]$ sẽ được xác định sớm hơn $n-1$ lần.

`*round`: duyệt qua tất cả các cạnh của đồ thị được lưu trong `edge list`.  
## III. Implement
Ta lưu đồ thị dưới dạng `edge list`, mỗi phần tử là `edge` với xuất phát từ $a$, đến $b$, và trọng số $w$.  
```cpp
for(int i=1; i<=n; i++)
    distance[i] = INF;
distance[x] = 0;
for(int i=1; i<=n-1; i++){
    for(auto e:edges){
        int a, b, w;
        tie(a, b, w) = e;
        distance[b] = min(distance[b], distance[a] + w);
    }
}
```
Hoàn toàn ta có thể thêm kĩ thuật truy vết để tìm ra các shortest path từ đỉnh $x$ đến $i$.  

### Kiểm tra mạch âm (Negative Cycle)
Nếu có mạch âm thì sau mỗi lần `round` ta luôn có thể giảm $distance[i]$ (cứ đi qua cái cạnh âm đó) thì khái niệm shortest path không tồn tại.  
Vì vậy, chỉ cần ta `round` thêm 1 lần mà phát hiện bất kì $distance[i]$ nào giảm giá trị thì đồ thị chứa **mạch âm**.

```cpp
for(auto e:edges){
    if (distance[a] + w < distance[b]) {
        cout << "Graph contains negative cycle!" << endl;
        break;
    }
}
```

**Tham khảo**: book_Antti Laaksonen.pdf
