---
title: 二叉树
date: 2020-07-08 18:08:09
tags: 二叉树
categories: 数据结构
---
### 二叉树基础概念

> 树结构的一种，子节点最多有两个，是算法，目录，索引常用数据结构，有搜索树，红黑树，平衡树，B+树


#### 完全二叉树

> 在完全二叉树中，除了最底层节点可能没填满外，其余每层节点数都达到最大值，并且最下面一层的节点都集中在该层最左边的若干位置。若最底层为第 h 层，则该层包含 1~ 2^h -1  个节点。



#### 二叉搜索树

> 有序的二叉树，先序遍历是从小到大的

- 若它的左子树不空，则左子树上所有结点的值均小于它的根结点的值；
- 若它的右子树不空，则右子树上所有结点的值均大于它的根结点的值；
- 它的左、右子树也分别为二叉排序树


<!-- more -->

#### 遍历顺序

> 总体分两种，一种深度遍历（前中后三个），一种层级遍历（一层一层遍历）

- 深度遍历，通过递归，迭代方式
	+ 前序遍历：根左右
	+ 中序遍历：左根右
	+ 后续遍历：左右根
- 层级遍历，一般使用队列实现，根据队列和栈的特性，一层一层遍历


``` Java
	// 前序遍历，按照根,右，左入栈，然后根左右出栈
	public static List<Integer> pre(Node head) {
        List<Integer> result = new ArrayList<>();
        if (head == null) {
            return result;
        }
        Stack<Node> stack = new Stack<>();
        stack.push(head);
        while (!stack.isEmpty()) {
            Node node = stack.pop();
            result.add(node.val);
            if (node.right != null) {
                stack.push(node.right);
            }
            if (node.left != null) {
                stack.push(node.left);
            }
        }

        return result;
    }

	// 中序遍历，先一直遍历到最左节点才能操作，所以不能像前序一样入栈出站，调整
    public static List<Integer> mid(Node head) {

        List<Integer> result = new ArrayList<>();
        if (head == null) {
            return result;
        }
        Stack<Node> stack = new Stack<>();
        Node cur = head;
        while (cur != null || !stack.isEmpty()) {
        	// 先把左侧入栈，直到最底部
            if (cur != null) {
                stack.push(cur);
                cur = cur.left;
            } else {
            	// 左和根出栈并把右节点入栈，右节点无子节点的话就出栈，有就迭代找左节点的底部
                cur = stack.pop();
                result.add(cur.val);
                cur = cur.right;
            }
        }

        return result;

    }

    // 跟前序遍历一样，不过只是调整入栈顺序为根左右，然后根右左出栈，最后翻转成左右根的顺序
    public static List<Integer> after(Node head) {

        List<Integer> result = new ArrayList<>();
        if (head == null) {
            return result;
        }
        Stack<Node> stack = new Stack<>();
        stack.push(head);
        while (!stack.isEmpty()) {
            Node node = stack.pop();
            result.add(node.val);

            if (node.left != null) {
                stack.push(node.left);
            }
            if (node.right != null) {
                stack.push(node.right);
            }
        }

        Collections.reverse(result);

        return result;

    }
```