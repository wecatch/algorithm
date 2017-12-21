title: 二叉查找树
date: 2017-11-28 13:10:49
tags:
categories:
- basic
---

# 概念

## 二叉查找树

二叉查找树首先是一颗二叉树，如果根节点的左子树不为空，则左子树所有节点的值都小于根节点，如果右子树不为空，则右子树所有值都大于根节点，二叉查找树的每一颗子树也是二叉查找树。

## 特性

- 中序遍历二叉查找树得到的序列是由小到大
- 二叉查找树的查找时间为 O(logn)
- 节点结构包括 {lchild,rchild,data,parent}

# 基本实现

```python
class Node(object):
    def __init__(self):
        self.lchild = None
        self.rchild = None
        self.data = None
        self.parent = None

```

采用链式存储结构

# 基本操作

