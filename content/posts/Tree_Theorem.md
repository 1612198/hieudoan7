+++ 
draft = false
date = 2019-11-02T23:12:30+07:00
title = "Tree's Theorem"
description = ""
slug = "" 
tags = []
categories = []
externalLink = ""
series = []
+++

# Tree's Theorem
01/11/2019

## I. Các cấu trúc cây cơ bản
### 1. Binary Tree
Mỗi node trong cây có tối đa $2$ con.  

### 2. Binary Search Tree
Là Binary Tree + giá trị của node luôn **lớn hơn** giá trị của node **con trái** và **bé hơn** giá trị của node **con phải**.  

### 3. AVL Tree (height balanced binary tree)
Là self balancing **Binary Search Tree** + **chênh lệch** giữa cây con trái và cây con phải tại bất kì node nào luôn **không vượt quá** $1$. (<=1)  

### 4. Red-Black Tree
Là self balancing **Binary Search Tree** +  
+ Không có 2 node RED liên tiếp  
+ Chiều cao đen luôn không đổi tại mọi node.  
+ tất cả node lá là BLACK  
+ Root always black (sometime this property is omitted)  
  
$\rightarrow$ Hệ quả: Path từ root đến lá xa nhất không dài hơn gấp $2$ lần path từ root đến lá gần nhất. (Dùng property $1$ để suy ra).

## II. So sánh AVL Tree vs Red-Black Tree
Giống nhau:  

+ Cả 2 đều là self balancing **Binary Search Tree**  
+ Các thao tác: Search, Insert, Delete đều mất chi phí $O(log(n))$.   

Khác nhau:  

+ AVL is more stricly balance than Red_black Tree  

$\rightarrow$ Suy ra

+ AVL search nhanh hơn  
+ AVL chèn lâu hơn  

Các CTDL như set, map đều dùng red-black tree (set chưa chắc :v), trong khi AVL dùng trong database (where fast look up required)

## III. Duyệt cây
```cpp
typedef struct Node* ref;
struct Node{
    int value;
    Node* left;
    Node* right;
};
```
### 1. Preorder
```cpp
//root-left-right
void preorder(ref root){
    if (root == NULL) return;
    cout << root.value << endl;
    preorder(root->left);
    preorder(root->right);
}
```
### 2. Inorder
```cpp
//left-root-right
void Inorder(ref root){
    if (root == NULL) return;
    Inorder(root->left);
    cout << root->value;
    Inorder(root->right);
}
```
### 3. Postorder
```cpp
//left-right-root
void Postorder(ref root){
    if (root == NULL) return;
    Postorder(root->left);
    Postorder(root->right);
    cerr << root->value;
}
```
### 4. DFS and BFS
```cpp
void dfs(int v, int p = 0){
    for(auto u:g[v]){
        if (u == p) continue;
        dfs(u, v);
    }
}
```