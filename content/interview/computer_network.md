+++ 
draft = false
date = 2019-10-28T21:27:43+07:00
title = "Computer Network"
description = ""
slug = "" 
tags = []
categories = []
externalLink = ""
series = []
+++

# Computer Network Questions for Interview.
28/10/2019

### 1. DNS là gì, hoạt động như thế nào?
DNS là Domain Name System: Hệ thống chuyển đổi domain name thành địa chỉ IP.  
host: yahoo.com -> search catch -> miss -> Resolver (ISP or nhà cc mạng: VIETEL (người đại diên)) ->  ROOT server -> .com server -> yahoo.com server -> đưa cho 1 IP server (rảnh) để mà về tải.
Remark: DNS using port 53.

2 video tham khảo: [1](https://www.youtube.com/watch?v=mpQZVYPuDGU) & [2](https://www.youtube.com/watch?v=2ZUxoi7YNgs) 

### 2. Có các loại mô hình mạng nào, nói chi tiết
Networking Model: OSI Model (7 layers) vs TCP/IP Model (4 layers)  
**OSI Model**  
1. Application   //Data Present  
2. Presentation  
3. Session  
4. Transport   //Data Flow -> segment (TCP/UDP)  
5. Network                 -> datagram (IP)  
6. Datalink                 -> Frame  
7. Physical                 -> Bit  

**TCP/IP Model**  
1. Application (1,2,3)  
2. Transport (4)  
3. Internet  (5)  
4. Network Access (6, 7)  

### 3. Switch vs Router
-> Switch < Router  
- Switch kết nối các host trong cùng 1 mạng  
- Router kết nối các mạng với nhau.  

### 4. TCP vs UDP 
**TCP**: Transport Control Protocol  
**UDP**: User Datagram Protocol  
TCP:  
+ Truyền đáng tin cậy  
+ Có tạo kết nối giữa bên gửi và bên nhận  
+ Dùng cơ chế pipeline (đảm bảo ko mất gói tin)  
(pipeline: set timeout, khi mà bên gửi ko nhận được ACK trong khoảng thời gian đó, nó sẽ tự động gửi lại)  
UDP:  
+ Truyền không tin cậy  
+ Không tạo kết nối  
+ Dùng trong các ứng dụng chịu lỗi được và cần tốc độ như media livestream.  