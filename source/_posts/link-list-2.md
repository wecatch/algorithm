title: 算法-链表
date: 2017-11-21 22:34:34
tags:
categories:
- basic
---

# 概念

## 链表

链表是线性表的一种，但是链表不同于顺序表，其在内存中的表示不一定是连续的地址空间，可以是任意的地址空间，基于此链表可以动态的添加任意大小的数据而不需要事先分配空间。

## 链表的分类

链表分为：单链表、双链表、循环链表，其元素存放的地址不连续，可以随意添加任意节点。

# 单链表

## 特性

- 节点的物理位置不一定连续，但是逻辑上通过指针实现连续
- 有一个头节点和一个尾节点，只有尾部节点没有后继节点
- 知道头部节点就可以遍历整个链表

单链表的表示（之后如果不特别说明默认都将使用 Python 来演示）:
```python

class Node(object):
    def __init__(self):
        self.data = None
        self.next = None
```

单链表的每个节点中都包含**一个数据域和指向下一个节点的指针域**，单链表通常有一个表头用来表示链表的开始，尾部节点的指针域为 null。

![单链表](http://ozoxs1p4r.bkt.clouddn.com/ll2.gif)


## 插入操作

插入新的节点 p

### 尾部插入

尾部插入节点，可以直接将尾部节点的指针域指向新节点 `tail.next = p`，如果尾部节点不知道需要遍历链表然后插入尾部节点

```python
cur = head
while cur.next:
    cur = cur.next
cur.next = p
    
```

### 在第 i 个位置之后插入

首先找到单链表的第 i 个节点位置，获取地址为 q，然后 `p.next = q.next, q.next=p`

## 删除操作

删除节点 p

删除单链表的节点非常简单，首先遍历找到节点，然后让节点的前驱节点指向节点的 next

```python
# 删除节点是头部节点
if head == p:
    head = head.next
    head.next = None
else:
    cur = head
    while cur.next != p:
        cur = cur.next
    cur.next = p.next
    p.next = None
```

## 查找指定节点

查找单链表很简单，遍历循环单链表即可

# 双链表

## 特性

每个节点包含两个指针域前驱 prev 和后继 next 以及一个数据域，但是头部节点没有前驱节点，尾部节点没有后继节点，单链表的表示：
```python
class Node(object):
    def __init__(self):
        self.data = None
        self.next = None
        self.prev = None
```

![double-link-list](http://ozoxs1p4r.bkt.clouddn.com/doubly_linked_list.jpg)


双链表的插入、查找、删除操作和单链表类似，只需要注意正确处理节点的前驱节点即可


# 循环链表

循环列表是尾部节点的指针域指向了头部节点形成了一个环，也就是说遍历循环列表时会回到头部节点，

![circle-link-list](http://ozoxs1p4r.bkt.clouddn.com/6140048350_a89768edc5.jpg)


# 链表的实际应用和操作

## 问题【1】

求一个单链表倒数第 m 个节点，要求不得求出链表长度，不得对链表进行逆转，找到则返回，没有则返回 null。


### 方法 1


![](http://ozoxs1p4r.bkt.clouddn.com/WX20171123-192334.png)

从图不难看出，可以设置两个变量a 和 b ，先让 b 开始从头遍历走 m 个节点，这样 a 和 b 的距离就相差 m 个节点，然后让 a 和 b 同时进行遍历，而倒数 m 个节点其实就是当 b 到达链表末尾的时候，a 指向的节点。

```python
a,b = head,head
p = 1
while b.next and p < m:
    b = b.next
    p += 1

while b.next:
    a = a.next
    b = b.next

```

## 问题【2】判断一个单链表是否有环

![](http://ozoxs1p4r.bkt.clouddn.com/de785741-d2de-431f-a9c6-ad964c3606b2_01.png)


单链表有环，也就是链表没有尾部节点，环一定存在于尾部


### 方法 1

链表有环说明链表从某个节点开始如果一直遍历，最终会回到那个节点，但是由于是单链表我们无法准确找到那个节点，因为只能单向遍历，除非把所有的结点都记录下来，也就是每遍历一个节点都需要记录和检查有没有被访问过，如果被访问过说明节点有环。

记录节点是否遍历过需要额外的空间，随着链表长度的变大而变大。

### 方法 2

如果有环，当在环上放置多个指针进行遍历，但是遍历的速度不同时，最终他们会相遇，由于不知道链表有多长，方法一在链表可能还没有遍历完就可能预定的存储空间不够用了，而用两个指针进行遍历相互追赶最终相遇却不需要额外的空间，注意最终二者相遇的时候肯定都不为 null

此文讲解了 https://lanphier.co.uk/algorithms/detecting-a-loop-in-a-linked-list/ 检测环的一些有趣探讨

```python
a, b = head, head
while a and a.next and b.next:
    a = a.next.next
    b = b.next
    if a == b:
        return a

return None

```

## 问题【3】

> 如果一个单链表有环，如何找出环的起始位置

## 问题【4】

> 如何判断两个单链表相交

![](http://ozoxs1p4r.bkt.clouddn.com/ll-intersection.jpg)

两个单链表相交意味着两个链表的尾部节点是相同的

### 方法 1

各自遍历两个链表，比对尾部节点是否相同，此方法时 O(max(n,m))，以两个链表在最长的为准


## 问题【5】逆转单链表

## 方法 1

单链表逆转其实就是把节点的 next 指针指向节点的前驱节点，现在假设链表需要反转，那么反转的过程其实会把链表截断，对于单链表来说，最重要的其实是表头，链表截断成两个之后，需要时刻记住两个链表分别的表头，语言反应了思维，所以变量的命名统一可以反应思维



```python
left_head = head
right_head = head.next
left_head.next = None
while left_head and right_head:
    tmp_right_head = right_head.next
    right_head.next = left_head
    left_head = right_head
    right_head = tmp_right_head

head = left_head
```

反转之后最后一步记住更改原来的表头

![step-1](http://ozoxs1p4r.bkt.clouddn.com/WX20171127-152340.png)

![step-2](http://ozoxs1p4r.bkt.clouddn.com/WX20171127-152421.png)

![step-3](http://ozoxs1p4r.bkt.clouddn.com/WX20171127-152508.png)


## 方法 2

另一种思路是提出一个假节点，称之为 dumb 节点

```python
def reverse():
    prev = None     # 假想的 prev 节点
    while head:
        temp = head.next
        # 反转，指向前驱
        head.next = prev
        # 移动 prev 和 head
        prev = head
        head = temp

    # 最后一个前驱节点也就是原来链表的尾部节点是新的头部节点
    head = prev
```