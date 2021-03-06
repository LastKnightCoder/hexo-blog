---
title: 二叉搜索树的第k大节点
categories: 剑指offer
tags:
	- 剑指offer
date: 2020-07-01
---


> 题目：给定一棵二叉搜索树，请找出其中第 $k$ 大的节点。例如，下图的二叉搜索树中，第 $3$ 大的节点是 $7$
>
> <img src="https://gitee.com/lastknightcoder/blogimage/raw/master/202007010947.svg"/>

其实解决这道题的算法十分的简单，因为对于二叉搜索树来说，它的中序遍历是排好序的结果，我们要查找第 $k$ 大的节点，只要中序遍历到第 $k$ 个节点即可，这个节点即是第 $k$ 大的节点

```java
public class GetKthNode {
    // 以全局变量表示还需遍历多少个节点
    private static int count;

    private static class BinaryTreeNode {
        BinaryTreeNode left;
        BinaryTreeNode right;
        int value;
        public BinaryTreeNode(int value) {
            this.value = value;
            this.left = null;
            this.right = null;
        }
    }

    public static BinaryTreeNode getKthNode(BinaryTreeNode root, int k) {
        if (root == null || k <= 0) {
            return null;
        }
        count = k;
        return getKthNode(root);
    }

    private static BinaryTreeNode getKthNode(BinaryTreeNode root) {
        BinaryTreeNode target = null;
        if (root.left != null) {
            target = getKthNode(root.left);
        }
        
        if (target == null) {
            if (count == 1) {
                target = root;
            }
            count--;
        }

        if (target == null && root.right != null) {
            target = getKthNode(root.right);
        }

        return target;
    }

    public static void main(String[] args) {
        BinaryTreeNode root = new BinaryTreeNode(8);
        root.left = new BinaryTreeNode(6);
        root.right = new BinaryTreeNode(10);
        root.left.left = new BinaryTreeNode(5);
        root.left.right = new BinaryTreeNode(7);
        root.right.left = new BinaryTreeNode(9);
        root.right.right = new BinaryTreeNode(11);

        BinaryTreeNode target = getKthNode(root, 3);
        System.out.println(target != null ? target.value : "null"); // 7
    }
}
```

