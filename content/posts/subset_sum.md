+++ 
draft = false
date = 2019-10-25T20:14:31+07:00
title = "All you need to know about Subset Sum Problem"
description = ""
slug = "" 
tags = []
categories = []
externalLink = ""
series = []
+++

# All You need to know about Subset Sum Problem  
25/10/2019

<u>Bài toán:</u> Cho 1 multiset $S = \\{a_1, a_2, ..., a_n\\}$, tìm 1 subset of $S$ thỏa điều kiện $P$.  
Naive Solution: $\rightarrow O(2^n * n)$  
Ta sinh tất cả các subset của $S$ $(O(2^n))$, sau đó duyệt qua từng subset $(O(n))$ để kiểm tra xem nó có thỏa điều kiện $P$ hay không.  

Nó thực sự là 1 bài toán NP-Complete nên ta chỉ có thể giải trong thời gian exponential. Có cải tiến $O(n^\frac{n}{2})$, xem [wiki](https://en.wikipedia.org/wiki/Subset_sum_problem)

Khi mà bài toán trở nên cụ thể (ie: Tập giá trị nhỏ), ta có thể dùng **DP Technique** để giải trong thời gian đa thức. Chúng ta cùng tìm hiểu 1 số bài toán cụ thể của **Subset Sum problem** sau đây:

## 1. 0-1 Knapsack Problem
<u>Bài toán:</u>  
Bạn có 1 balo (Knapsack) với sức chứa $W$ kilograms.  
Có $n$ món đồ có trọng lượng $\\{w_1, w_2, ..., w_n\\}$ và có giá trị tương ứng $\\{v_1, v_2, ..., v_n\\}$.  
Hãy tìm cách lấy các món đồ nào đó để bỏ vừa trong balo và đạt giá trị cao nhất.  

Khi mà bài toán có tính lặp lại nhỏ hơn (recursion, suboptimal problem) và Overlap (hiển nhiên đến sau) thì đó là lúc ta dùng **DP Technique**.  

<u>Solution:</u> (**DP Technique**)  
Đặt $dp(n, W)$ là maximum value we can get from $n$ món đồ và với cái balo có capacity of $W$ kilograms. Thì:  
$$ dp(n, W) = max\begin{cases}dp(n-1, W) & (not\ taken)\\\ v[n] + dp(n-1, W - w[n]) & (taken)\end{cases}$$  
Base case: (Vẽ bảng ra cho rõ)  
+ $dp(0, j) = 0\ \ \ \forall 0\leq j \leq W$  
+ $dp(i, 0) = 0\ \ \ \forall 1\leq i \leq n$ 

<u>Implementation:</u>  
```cpp
//top-down approach
int n, W, w[N], v[N];
int mem[N][W]; //memset(mem, -1, sizeof mem);
int dp(int n, int W){
    if (W <= 0 || n == 0) return 0;
    int &res(mem[n][W]);
    if (res != -1) return res;
    return res = max(dp(n-1, W), v[n]+dp(n-1, W - w[n]));
}

//bottom-up approach
int dp[N][W]; //full 0
for(int i=1; i<=n; i++) {
    for(int j=1; j<=W; j++) {
        dp[i][j] = max(dp[i-1][j], v[i]+dp[i-1][j-w[i]});
    }
}
```
Complexity: $$\rightarrow O(nW)$$
Note: Guessing? Is the last element was taken or not? :v  
## 2. Existence of a subset Equal to given Sum
<u>Bài toán </u>  
Given a set of non-negative integers $S$, and a value $SUM$, determine if there is a subset of $S$ with sum equal to given $SUM$.  

<u>Solution</u>  (**DP Technique**)  
Why DP? Giả sử ta đang ở trạng thái lơ lửng $i$ (đang duyệt tới phần tử thứ i (based 1)), nếu $i-1$ phần tử trước đó đã có thể construct a subset with sum $SUM$ thì tới đây chắc chắn có mà không cần đếm xỉa tới $a[i]$, nếu trước đó chưa có nhưng nó có subset with sum equal $SUM-a[i]$ thì giờ thêm $a[i]$ vào subset đó 100% perfect. Cảm quan như vậy dẫn đường cho DP.  
$\rightarrow$ **Recall**: Những bài subset thường là tới phần tử $i$ ta dựa vào tất cả những cái subset đã cấu thành tử $i-1$ phần tử trước đó và thêm vào, + với nó đứng 1 mình (tương đương với thêm vào tập rỗng $i-1$ phần tử trước đó).  

Đặt $dp(i, j)$ là có hay không subset of i phần tử đầu với sum bằng $j$.  
Rõ ràng:  
$$dp(i, j) = dp(i-1, j-a[i])\ |\ dp(i-1, j)$$
base case: $dp(i, 0) = true \ \ \ \forall 1\leq i\leq n$  

<u>Implementation</u>
```cpp
int dp[N][SUM]; //int ms bitwise or duoc
for(int i=0; i<=n; i++) dp[i][0] = true;
for(int i=1; i<=n; i++){
    for(int j=0; j<=SUM; j++){
        dp[i][j] = dp[i-1][j];
        if (j-a[i]>=0) dp[i][j] |= dp[i-1][j-a[i]];
    }
}
bool ans = dp[n][SUM];
//Link Submission: https://practice.geeksforgeeks.org/problems/subset-sum-problem/0
```
$\rightarrow$ Bài toán có thể modify thành bài toán kiểm tra sự tồn tại của subset equal 0 với set gồm các phần tử không cần phải non-negative.  

### 2.1. Parition Problem
<u>Bài toán</u>:  
Cho 1 multiset of integers $S$, the task is divide it into $2$ set $S1$ and $S2$ such that the absolute difference between their sums is minimum.   

$\rightarrow$ Thật ra đây chỉ là bài giống bài trên, ta check xem cái $sum$ nào có thể construct gần với $mid$ nhất là ok.  
<u>Solution</u>  
Dùng DP như trên, chỉ khác ở bước kết quả, ta duyệt từ mid về trước để tìm cái $dp[n][i] = true$ đầu tiên thôi.  
<u>Implementation</u>  
```cpp
int n, a[N], sum = 0;
int all_sum;
for(int i=1; i<=n; i++) sum += [i];
all_sum = sum;
sum /= 2;
vector<vector<int>> dp(n+1, vector<int>(sum+1, 0));
for(int i=0; i<=n; i++) dp[i][0] = 1; //empty set
for(int i=1; i<=n; i++){
    for(int j=0; j<=sum; j++){
        dp[i][j] = dp[i-1][j];
        if (j-a[i]>=0) dp[i][j] |= dp[i-1][j-a[i]];
    }
}
int ans;
for(int i=sum; ; i--){
    if (dp[n][i] == true){
        ans = all_sum - i - i;
        break;
    }
}
cout << ans << endl;
//Link submission: https://practice.geeksforgeeks.org/problems/minimum-sum-partition/0
```

