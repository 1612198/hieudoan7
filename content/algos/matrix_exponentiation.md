+++ 
draft = false
date = 2020-01-22T13:57:57+07:00
title = "Matrix Exponentiation"
description = ""
slug = "" 
tags = []
categories = []
externalLink = ""
series = []
+++

# Matrix Exponentiation
22/01/2020

## I. Linear Equation
Đề bài: Giải hệ thức đệ quy:  
$$\begin{cases}f_i = c_0 + c_1 f\_{i-1} + c_2 f\_{i-2} + ... + c_k f\_{i-k}\\\f_0, f_1, ..., f\_{k-1}\end{cases}$$

Giải:  
Ma trận xuất phát điểm: $F_k$ (k+1) x 1

$$\begin{bmatrix}f_0 \\\f_1 \\\ ...\\\f\_{k-1} \\\1 \end{bmatrix}
$$

Ma trận chuyển tiếp: (Transition) $T_{(k+1).(k+1)}$  

$$\begin{bmatrix}0 & 1 & 0 & ... & 0 & 0 \\\0 & 0 & 1 &...& 0 & 0 \\\ ... \\\c_k & c\_{k-1} & c\_{k-2} & ... & c_1 & c_0\end{bmatrix}$$

Ta dễ thấy  
$$T.F\_k = \begin{bmatrix}f_1 \\\f_2 \\\ ...\\\f\_{k} \\\1 \end{bmatrix}$$

$$T.F\_k = F\_{k+1}$$
Cứ như vậy, để tính $f_n$ ta cần tính $F_n = T.F\_{n-1}$  
## II. Variation
Thêm $i^2$, $i$, $1$.  
Ví dụ:   
$$f\_i = f_{i-1} + 2i^2 + 3i + 5$$
Ma trận xuất phát điểm:  $F_k$  
$$\begin{bmatrix}f_0 \\\1 \\\ 1 \\\1 \end{bmatrix}
$$  
Ma trận chuyển tiếp: $T$
$$\begin{bmatrix}1 & 5 & 3 & 2\\\0 & 1& 0& 0 \\\0&1&1&0\\\0&1&2&1 \end{bmatrix}$$  
Lấy $T$ nhân với $F$ sẽ thấy sự đặc biệt.  
Lưu ý: Sau $f_0$ thì lần lượt là $1$, $i$ và $i^2$.  



### III. Reference
1. [Blog codeforces](https://codeforces.com/blog/entry/67776)