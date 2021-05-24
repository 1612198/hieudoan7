+++ 
draft = false
date = 2019-10-29T23:11:03+07:00
title = "Operator System"
description = ""
slug = "" 
tags = []
categories = []
externalLink = ""
series = []
+++

# Operator System Questions for Interview
29/10/2019

Hệ điều hành là 1 chương trình quản lý phần cứng và cấp phát tài nguyên một cách phù hợp cho các chương trình.  

## I. Process (Tiến trình)
### 1. Tiến trình, tiểu trình?
Tiến trình là một chương trình máy tính đang thực thi.  
Tiểu trình (thread) là một đơn vị thực thi độc lập mà CPU có thể điều phối được. Thông thường, một **Tiến trình** gồm nhiều **Tiểu trình**.  
Khi 1 tiến trình $P$ gồm các tiểu trình $T_1$, $T_2$, $T_3$. Thì giữa các tiểu trình này khác nhau:  
+ Thread ID  
+ Register  
+ Stack  
Và dùng chung:  
+ Code  
+ Data (Global variable)  
+ File  
+ Heap  

Bỏ nhỏ: Child process là 1 process được sinh ra từ 1 process khác (parent process) mà thôi, sau đó nó lớn sao lớn và nó chết trước cha nó :v

### 2. Context Switch
$\rightarrow$ Chuyển đổi ngữ cảnh là một quá trình lưu lại trạng thái hiện hành và chuyển sang trạng thái khác (nạp mới hoặc phục hồi trạng thái cũ được lưu trước đó)  
$\rightarrow$ Chuyển đổi ngữ cảnh là đặc tính tiêu biểu và nhất thiết phải có của một hệ điều hành đa nhiệm (multitasking)

#### Context Switch xảy ra khi nào?
Khi một process đang thực thi, có 2 trường hợp mà hệ thống phải thực hiện context switch.  
1. Preemptive: Bị chiếm bởi process khác có độ ưu tiên cao hơn.  
2. Non-preemtive: Đang chờ 1 tác vụ nhập xuất gì đó.

### 3. Kernel Space vs User Space
Xét trên RAM: (Memory)  
Có 1 phần dành riêng để chạy kernel (OS Background process) gọi là kernel space: full access phần cứng và quản lý các ứng dụng đang chạy trên User space.  
User space: các ứng dụng người dùng load lên đó chạy, thực ra chạy trên virtual memory.  

Để truy xuất thủ tục trong kernel space, ta cần phát sinh 1 trong 3 event:  
+ Ngắt (interrupt): Hết đồng hồ, ngắt từ I/O device   
+ Exception: divide by 0  
+ Trap (software interrupt): system call, break point  

## II. Memory
Coi cái sơ đồ hoạt động sau đây.
![virtual_memory](/imgs/virtual_memory.jpg)

Các vấn đề xoay quanh.  
#### 1. Virtual Memory có kích thước bao nhiêu?
Số bit biểu diễn virtual address chính là kích thước (conceptional) của virtual memory. > số bit của physical address.  
Vd: 32-bit physical address, 48-bit virtual address

#### 2. Virtual Memory được lưu ở đâu?
Kì thực, nó không hề tồn tại, nên không có lưu ở đâu hết.

#### 3. Công dụng của Virtual Memory?
- Giải quyết được phân mảnh
- Chương trình có kích thước > RAM vẫn chạy được
- Người lập trình ko cần quan tâm đến kích thước vùng nhơ RAM

#### 4. Page Table được lưu ở đâu?
Page Table nó được lưu trong 1 thiết bị phần cứng **MMU** (Memory Management Unit) tích hợp trong CPU. MMU luôn đi kèm với **TLB** (Translation Lookaside Buffer, nhiệm vụ catching).  
![minh_hoa](https://upload.wikimedia.org/wikipedia/commons/thumb/d/dc/MMU_principle_updated.png/1024px-MMU_principle_updated.png)
Sơ đồ hoạt động của TLB.  
![hoad_dong_TLB](https://upload.wikimedia.org/wikipedia/commons/c/c1/Steps_In_a_Translation_Lookaside_Buffer.png)

#### 5. Làm gì khi xảy ra page fault?
Khi mà search trong page table thấy bit invalid, nghĩa là chưa được load lên RAM, thì nó phát sinh Trap (ngắt mềm) cho OS, hệ điều hành mới block tiến trình lại và tiến hành đi load ô nhớ đang cần từ disk lên RAM để tiếp tục chạy. Tiến hành các bước sau:  
+ Chọn frame để thay thế (nạn nhân, thuật toán thay trang)  
+ Lưu nạn nhân vào disk. (cải tiến là thêm dirty bit, nếu fram đó ko có gì thay đổi thì khỏi cần tải về disk)  
+ Cập nhật bảng trang (gắn invalid cho nạn nhân)  
+ Đọc trang cần thiết vào frame vừa chọn  
+ Cập nhật valid cho trang đó  
+ Chạy lại instructor lúc nãy.  

#### 6. Làm sao OS biết page nào trong disk?
Có, `swap_info_struct()` trong kernel á, nếu muốn rõ hơn thì tải code kernel về đọc. [link stackOverFlow](https://stackoverflow.com/questions/32846456/how-does-the-os-know-disk-address-of-an-absent-page)  

#### 7. Các thuật toán thay trang?
+ NRU: Not Recently Used
+ LRU: Least Recently Used
+ FIFO: First In First Out  
    ++ Cơ hội thứ 2: thêm 1 bit active và nếu đầu queue nhưng mà có active thì cho xuống cuối queue đứng  
    ++ Clock: thay vì cứ cắt đầu xuống đuôi thì nối mẹ vòng rồi cho kim đồng hồ quay.

#### 8. Một tiến trình chạy như thế nào?
Nó load hết lên Virtual Memory, đây chỉ là mang tính conceptional chứ làm gì tồn tại virtual memory, nghĩa là nó load từng ô nhớ thôi, chạy cần cái nào nó load cái đó nhờ `swap_info_struct()` trong OS.  
![swap](/imgs/swap.jpg)
Thế nhiều tiến trình chạy như thế nào, thì nó cứ load hết lên virtual memory theo thứ tự từng tiến trình, vì VM ko tồn tại dẫn đến thực tế là cứ tiến trình nào cần ô nhớ nào thì nó ms load lên RAM thôi.  
