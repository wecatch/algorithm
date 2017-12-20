title: 算法-二叉树
date: 2017-11-28 13:10:49
tags:
categories:
- basic
---

# 概念

## 二叉树

二叉树是树的度为 2 的树，而且二叉树的每个节点子树是有顺序的，分别是左子树和右子树，二叉树是有序树

# 二叉树的实现

二叉树的实现分为：顺序存储结构和链式存储结构。

## 顺序存储结构

利用二叉树自身的一些性质来推断出其左右子树在顺序存储结构中的位置，由于顺序存储结构是以二叉树最大作为空间估算前提条件，对空间的利用不合理。


## 链式存储结构

在二叉树的链式存储结构中，每个节点都包含数据域、左右子孩子的链接。
```python
class Node(object):
    def __init__(self):
        self.data = None
        self.left = None
        self.right = None
```


# 二叉树的基本操作

## 遍历二叉树

二叉树的遍历是对二叉树中每个节点进行访问，完整遍历一棵树就是要遍历二叉树的根节点、左右子树。

### 前序遍历

根节点->左子树->右子树
```python
def traverse(node):
    if node is None:
        return
    print(node.data)
    traverse(node.left)
    traverse(node.right)

```


### 中序遍历

左子树->根节点->右子树
```python
def traverse(node):
    if node is None:
        return
    traverse(node.left)
    print(node.data)
    traverse(node.right)

```

### 后序遍历

左子树->右子树->根节点
```python
def traverse(node):
    if node is None:
        return
    traverse(node.left)
    traverse(node.right)
    print(node.data)

```


### 层序遍历

层序遍历又称广度优先搜索，按照从上到下、从左到右的顺序对节点按照节点所在的层进行遍历，借助队列实现。

```python

def layerorder(node):
    queue = list()
    path = list()
    queue.append(node)
    while len(queue):
        current = queue.pop(0)
        path.append(current.val)
        if current.left:
            queue.append(current.left)
        if current.right:
            queue.append(current.right)
```

# 二叉树的实际应用和操作

## 问题【1】求一个节点的父节点

二叉树是单向的层次结构，无法直接由子节点获取父节点，但是我们可以根据层序遍历的思想获得父节点

```python

def find_parent(root, node):
    queue = []
    queue.append(root)
    while len(queue):
        current_node = queue.pop(0)
        if current_node.left:
            queue.append(current_node.left)
            if current_node.left == node:
                return current_node
        if current_node.right:
            queue.append(current_node.right)
            if current_node.right == node:
                return current_node

```


## 问题【2】求一个节点的兄弟节点

由于二叉树的单向的层次结构，获取兄弟节点可以借助先获取父节点然后再通过父节点获取兄弟节点

```python

find_parent(root, node).left
find_parent(root, node).right

```

## 问题【3】求一个二叉树的高度

### 方法 1

递归的去遍历所有节点求树高度，最后就演化成只要求根节点子树的高度对比就行，非常容易实现

```python

def get_tree_depth1(node, depth=0):
    """
    递归的写法
    """
    if node is None:
        return depth
    depth += 1
    left_depth = get_tree_depth1(node.left, depth)
    right_depth = get_tree_depth1(node.right, depth)

    return max(left_depth, right_depth)

```


### 方法 2

如果树的高度非常高，会导致递归的层次非常深，可以改成循环.

二叉树的高度也就是二叉树的深度，等于二叉树的叶子节点的最大深度，这里我们需要对二叉树进行深度遍历来获得二叉树的节点深度和最大深度，鉴于此我们在遍历过程中需要进行回退，因为事先我们是不知道哪一条遍历的路径是最长的，所以需要借助栈这种可以回退的结构。

