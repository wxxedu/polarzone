---
title: "Binary Search Tree"
date: 2022-03-09T15:58:07+08:00
draft: false
series: ["CS2040S"]
tags: ["CS2040S", "Notes", "Data Structures", "Algorithms", "Binary Search Tree", "Tree"]
---

Recall that in binary search, we find the middle element of a list, compare the element that we want to find with the middle element, and then chooses to go to the left part of the list or the right part. A binary search tree is using a similar approach. The difference here is that in a binary search tree, we store the elements in a tree structure.

Say that we have a list that looks like this:

```
1 2 3 4 5 6 7
```

A binary search tree for this set of number could be like:

![Binary Search Tree](/static/CS2040S/binary-search-tree-01.svg)

where we would call each element in the tree a node. The top-most node (colored in pink) here is called the root, whereas nodes without children (colored in light blue) are called leaves.

Note that with regards to a binary search tree, we have it satisfy the following rule:

- A node can have at most two children, a left child and a right child;
- The left child is less than the node, and the right child is greater than the node. 

We have to make sure that this rule is maintained while inserting a node. To do this, during the insertion of a node, we:

1. compare the value that we insert with the value at the current node
	1. if the value is less than the value of the current node, we set the current node to the **LEFT** child of the current node;
	2. if the value is greater than the value of the current node, we set the current node to the **RIGHT** child of the current node;
2. we repeat this process untill current node is null. It is where we should insert a node. 

For example, if we were to insert $0$ into the chart above, we first look at $4$. Since $0 < 4$, we should go left. 

![Binary Search Tree](/static/CS2040S/binary-search-tree-02.svg)

Then we shall look at $2$, since $0 < 2$, we shall go left once more. 

![Binary Search Tree](/static/CS2040S/binary-search-tree-03.svg)

Since $0<1$, we shall go left once more. We realize that $1$ does not have a left child, it means that we should insert $0$ at that location.

![Binary Search Tree](/static/CS2040S/binary-search-tree-04.svg)

## Basic Features

- Insert: inserting a node takes $O(\log n)$ on average. However, if we get unlucky, our BST will end up looking like a linked list, which would take $O(n)$;
- Delete: deleting a node takes $O(\log n)$ on average. The same thing could be said with unlucky cases;