+++ 
draft = false
date = 2019-07-07T14:27:00+07:00
title = "Z-function"
description = ""
slug = "" 
tags = []
categories = []
externalLink = ""
series = []
+++
<style>
    th, td {
        padding: 1.0rem;
    }
    table, th, td {
      border: 2px solid black;
    }
 </style>
# String Processing
18/04/2019  

Cho 2 xâu P (pattern) và S (string). Mộ số bài toán  

- Tìm vị trí xuất hiện đầu tiên của P trong S
- Đếm số lần xuất hiện P trong S
- Cho S, tìm P ngắn nhất để repeat P nhiều lần ta được S
- ...

## I.  Z-function
### 1. Định nghĩa Z-function
Cho xâu $S = s[0..n-1]$.  
Hàm $Z$ của xâu $S$ là dãy $z[0], z[1], ..., z[n-1]$ trong đó $z[i]$ là **max (length) prefix** của $s[i..n-1]$ với $s[0..n-1]$.  
Ví dụ: Xét xâu $S = 'abaabab'$ (length = 7)  
Ta sẽ có được hàm z

<center>

 i | 2 xâu cần so sánh | z[i] 
---|-------------------|------
 0 | quy ước = 0       |  0   
 1 | 'abaabab' vs 'baabab' | 0
 2 | 'abaabab' vs 'aabab'  | 1
 3 | 'abaabab' vs 'abab'   | 3
 4 | 'abaabab' vs 'bab'    | 0
 5 | 'abaabab' vs 'ab'     | 2
 6 | 'abaabab' vs 'b'      | 0

</center>
Sau khi có được hàm Z của xâu  S, thì với $z[k] = x$ cho ta biết: 

- $x$ kí tự đầu của $S$ sẽ xuất hiện lại ở vị trí $k$
- Số phần tử có giá trị $x$ là số lần xuất hiện của $S[0..x-1]$ trong $S$.

### 2. Implement
#### a) Thuật toán bình thường
```cpp
z[i] = 0 for all i
for(int i=1; i<n; i++){
    while(i+z[i] < n && s[z[i]] == s[i+z[i]]) 
        z[i]++;
}
```
$ \Rightarrow O(n^2)$

#### b) Thuật toán cải tiến
**Ý tưởng**: Ta sẽ bám vào thằng xa nhất mà ta đã nhìn thấy.  
Như thế nào là thằng xa nhất ta đã nhìn thấy?  

Ví dụ: ta đang tính $z[i]$ được $k$ thì thằng xa nhất mà ta có thể nhìn thấy là $i+k-1$.  
Gọi thằng xa nhất mà ta có thể nhìn thấy là $right\\_most$ thì  
$$ right\\_most = max(j+z[j]-1) \hspace{9mm} \forall  0 \leq j \leq current\ i $$ 
Từ thằng $right\\_most$ ta sẽ xác định được điểm xuất phát của nó là $left$ (ta có lưu lại mà). Nghĩa là mình bám đoạn $left$ đến $right\\_most$ (length = k). (điều chắc chắn là $i > left$).  

Có phải là đoạn $s[i..right\\_most]$ nó hoàn toàn giống với đoạn $s[i-left..k-1]$ vì $z[left]
= k$. Vì vậy ta sẽ tận dụng việc đoạn này đã được so sánh prefix với $s$ trước đó thông qua việc đi tính $z[i-left]$ trước đó.  
 
Đọc code sẽ rõ, nếu may mắn full cây (trùng cả đoạn) thì tăng lên so sánh tiếp, không thì được 1 khúc trùng với $z[i-left]$.  

```cpp
for(int i=1; i<=n; i++){
    if (i <= right_most) z[i] = min(z[i-left],right_most-i+1);
    while(i+z[i] < n && s[z[i]] == s[i+z[i]]) 
        z[i]++;
    if (i+z[i]-1 > right_most){
        left = i;
        right_most = i+z[i]-1;
    }
}
```
$ \Rightarrow O(n)$

I think this is good link for you to discover Z-function: https://cp-algorithms.com/string/z-function.html