+++ 
draft = false
date = 2019-11-02T14:11:28+07:00
title = "Sorting"
description = ""
slug = "" 
tags = []
categories = []
externalLink = ""
series = []
+++
# Sorting Algorithm
02/11/2019  

This post is going to give you some interesting algorithm ralating sorting elements.  

**Problem** Sorting an array $a$ consists of $n$ elements in increasing order.  

## I. $O(n^2)$ Algorithm
### 1. Interchange Sort
```cpp
for(int i=0; i<n; i++){
    for(int j=i+1; j<n; j++){
        if (a[j] < a[i]) swap(a[j], a[i]);
    }
}
```

### 2. Bubble Sort
Từng phần tử nổi lên như bong bóng vậy đó.  
```cpp
for(int i=n-1; i>0; i--){
	    for(int j=1; j<=i; j++){
	        if (a[j] < a[j-1]) swap(a[j], a[j-1]);
	    }
	}
```
Ngoài ra còn có Insertion Sort, Selection Sort, Shaker Sort.  

## II. $O(n\ log(n))$ Algorithm
### 1. Quick Sort
**Ý tưởng**  

+ Chọn 1 phần tử cần sort (pivot)  
+ Sort phần tử đó: partition  
+ Quick sort cho 2 bên pivot  

**Cài đặt**  
Tham khảo [GFG](https://www.geeksforgeeks.org/quick-sort/), chọn phần tử cuối làm pivot cho dễ cài đặt.  
```cpp
int partition(int l, int r);
void quick_sort(int l, int r){
    if (l >= r) return;
    pi = partition(l, r);  //correct pos of pivot
    quick_sort(l, pi-1);
    quick_sort(pi+1, r);
}
int partition(int l, int r){
    int pivot = a[r];
    int i = l; //index of elements < pivot
    for(int j=l; j<=r-1; j++){
        if (a[j] < pivot){
            swap(a[j], a[i]);
            i++;
        }
    }
    swap(a[i], a[r]);
    return i;
}
```
**Lưu ý**  
Quick Sort có độ phức tạp trung bình là $O(n\ log(n))$ và **worst case** là $O(n^2)$ khi mảng đã được **sort** (kể cả tăng hay giảm).  

### 2. Merge Sort
**Ý tưởng**  

+ Chia mảng thành 2 phần left, right  
+ Sort bên trái, Sort bên phải  
+ Merge 2 bên lại.  

Ý tưởng **divide and conquer** (DP), thao tác chính là thao tác `merge` đó mới chính là lúc sort thực sự.  

**Cài đặt**
Tham khảo [GFG](https://www.geeksforgeeks.org/merge-sort/)  
```cpp
//a[] là biến toàn cục rồi
void merge(int l, int m, int r);
void merge_sort(int l, int r){
    if (l >= r) return; //= is maximal roi
    int m = (l+r)/2;
    merge_sort(l, m);
    merge_sort(m+1, r);
    merge(l, m, r);
}
void merge(int l, int m, int r){
    int n1 = m-l+1;
    int n2 = r-m;
    int L[n1], R[n2]; //temp
    for(int i=0; i<n1; i++){
        L[i] = a[l+i];
    }
    for(int i=0; i<n2; i++){
        R[i] = a[m+1+i];
    }
    int i = 0, j = 0, k = l;
    while(i<n1 && j<n2){
        if (L[i] <= R[j]) a[k++] = L[i++];
        else a[k++] = R[j++];
    }
    while(i<n1)
        a[k++] = L[i++];
    while(j<n2) 
        a[k++] = R[j++];
}
```
$\rightarrow$ Merge Sort luôn có độ phức tạp $O(n\ log(n))$. 
## III. O(n) Algorithm
### 1. Counting Sort
Khi mà mảng chỉ gồm các small elements thì ta có thể dùng Counting Sort.  
**Ý tưởng**  
Nếu phần tử $x$ trước nó có $y$ phần tử nhỏ hơn thì nó phải nằm ở vị trí $y$ (based 0).  
**Cài đặt**
```cpp
int cnt[M]; //M = *max_element(a, a+n)
memset(cnt, 0, sizeof cnt);
for(int i=0; i<n; i++){
    cnt[a[i]]++;
}
for(int i=1; i<M; i++){
    cnt[i] += cnt[i-1];
}
int ans[n];
for(int i=0; i<n; i++){
    ans[--cnt[a[i]]] = a[i];  //base 0
}
for(int i=0; i<n; i++) a[i] = ans[i]; //copy answer
```

Còn 1 số thuật toán fast khác như **Radix Sort**, **Flash Sort**.