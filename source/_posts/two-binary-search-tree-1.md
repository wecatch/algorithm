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
- 节点结构包括 {lchild,rchild,key,value,N}

二叉查找树的查找和插入几乎都是1.39logN

# 基本实现

```python
class Node(object):
    def __init__(self):
        self.lchild = None
        self.rchild = None
        self.key = None
        self.value = None
        # 节点计数器，以该节点为根的子树的节点总数，包含该节点
        self.N = 0 

class BST(object):

    def get(self, node, key):
        """
        查找
        """
        pass
    
    def put(self, key, val):
        """
        插入
        """
        pass

    def min(self, node):
        """
        最小
        """
        pass

    def max(self, node):
        """
        最大
        """
        pass

    def floor(self, node, key):
        """
        小于等于 key 的最大值
        """
        pass

    def ceil(self, node, key):
        """
        大于等于 key 的最小值
        """
        pass

```

节点计数等式成立
```
size(x) = size(x.left) + size(x.right) + 1

```

采用链式存储结构

# 基本操作


## 查找

二叉查找树的递归查找很容易实现

```python
def get(self, node, key):
    if node is None:
        return None
    if key < node.key:
        return self.get(node.left, key)
    if key > node.key:
        return self.get(node.right, key)
    return node.value
```

根据比较返回指定 key 在左子树或右子树的位置

## 插入

```python

def put(self, node, key, val):
    # 如果树为空，生成新的节点
    if node is None:
        return Node(key, val, 1)
    if key < node.key:
        node.left = self.put(node.left, key, val)
    elif key > node.key:
        node.right = self.put(node.right, key, val)
    else:
        node.val = val
    node.n = self.size(node.left) + self.size(node.right) + 1
    return node

```

插入操作中，如果树是空的，则返回包含有该健值对的新节点，如果小于当前节点，则插入到左子树，大于当前节点插入到右子树，在插入中每个递归的返回都是要重置路径上的每个父节点指向的子节点的连接，这是因为插入的新节点会导致已有的节点的左右连接发生变化。实际上在一个比较简单的二叉树中**唯一更新的连接就是最底层指向新节点的连接**，更上层连接的重置，可以通过比较来避免。


## 最小键

如果根节点的左子树为空，最小键就是根节点，如果左子树非空，最小键就是左子树的最小键

```python
def min(self, node):
    """
    左子树中的最小的节点，也就是最左的节点
    """
    if node is None:
        return
    if node.left:
        return self.min(node.left)
    else:
        return node

```


## 最大键


如果根节点的右子树为空，最大键就是根节点，如果右子树非空，最大键就是右子树的最大键

```python
def max(self, node):
    """
    右子树中的最大的节点，也就是最右的节点
    """
    if node is None:
        return
    if node.right:
        return self.max(node.right)
    else:
        return node

```


## 向下取整 floor

想要查找 floor(G)，小于等于 G 的最大键，如果 G 小于二叉查找的根节点，floor(G) 一定在根节点的左子树中，如果 G 大于根节点，只有在根节点的右子树中存在小于 G 的节点时，floor(G) 才会出现在右子树中，否则根节点就是 floor(G)

```python
def floor(self, node, key):
    """
    小于等于 key 的最大键, key 向下取整
    """
    if node is None:
        return
    # 当前节点大于 key
    if key < node.key:
        return self.floor(node.left, key)

    # 当前节点等于 key
    if key == node.key:
        return node

    # 当前节点小于 key 右子树中找大于 key 的键，因为右子树中的所有键小于当前节点
    # 右子树中存在小于 key 的键
    t = self.floor(node.right, key)
    # 如果右子树不存在小于 key 的键，说明当前小于 key 的节点是那个 floor(key)，因为右子树的键都大于当前节点
    if t is not None:
        return t
    else:
        return node

```

## 向上取整 ceil

想要查找 ceil(G)，大于等于 G 的最小健，如果 G 大于二叉查找的根节点，ceil(G) 根节点的右子树中，如果 G 小于根节点，只有在根节点的左子树中存在大于 G 的节点时，ceil(G) 才会出现在左子树中，否则根节点就是 ceil(G)


```python

def ceil(self, node, key):
    """
    大于等于 key 的最小键, key 向上取整
    """
    if node is None:
        return

    # 当前节点小于 key
    if key > node.key:
        return self.ceil(node.right, key)

    # 当前节点等于 key
    if key == node.key:
        return node

    # 当前节点大于 key在左子树中找大于 key 的键，因为左子树中的所有键小于当前节点
    # 左子树中存在大于 key 的键
    t = self.ceil(node.left, key)
    # 如果不存在，说明当前小于 key 的节点是那个 ceil(key)
    if t is not None:
        return t
    else:
        return node


```

## 删除最小键


## 删除最大键




