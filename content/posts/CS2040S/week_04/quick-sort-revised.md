---
title: "Quick Sort Revised"
date: 2022-02-16T18:42:50+08:00
draft: false
series: ["CS2040S"]
tags: ["CS2040S", "Notes", "Data Structures", "Algorithms", "Sorting"]
---

In the [previous article](./../quick-sort), I discussed the brief idea behind quicksort. However, I wasn't clear about the details, therefore creating much trouble for my own implementation of the quick sort. This article will build on top of the ideas discussed in the previous article. More specifically, it will focus more on the details of the implementation of quick sort. Specifically, we shall be discussing quicksort with 3-way partitioning. 

I also have to give a shout out to my friends Anis and Bruce. Anis wrote a C version of quicksort for me, and Bruce called me after seeing my last article, teaching me the better way of looking at and implementing the quicksort algorithm. 

## Closed-Open Intervals

In an in-place quick sort, we will be dividing the list to be sorted into different partitions. Because we will be using the indexes to keep track of the partitions of the quick sort, it would be great if we could use a fixed system of describing the lists -- to avoid chaos. We shall use intervals that are closed on the left and open on the right. For example, the interval 

$$[a,b)$$

represents all the numbers that are greater than or equal to $a$ and strictly less than $b$. In other words, 

$$a \leq x < b.$$

In our case here, because we shall be describing the indices of elements in a list, we are representing integers. So, for each partition of the list, if we say that its two ends are $a$ and $b$, then we mean that the list starts at index $a$ (inclusive) and ends at $b-1$ (inclusive). 

## 3 Way Partitioning

### Review of Procedure

As discussed in the [previous article](Study%20Notes/Class%20Notes/Y1S2/CS2040S/Lecture%20Notes/Week%2005/Quick%20Sort.md), the procedure of quicksort can be summarized as:

1. find a random pivot point
2. partition the list into three parts according to the pivot point:
	1. The left partition: the elements that are smaller than the pivot value,
	2. The middle partition: the elements that are equal to the pivot value,
	3. The right partition: the elements that are larger than the pivot value.
3. repeat this process

### Partitioning

Now, assume that we are in the process of sorting out the array, we shall realize that the list is divided into 4 segments:

$$
\underbrace{s_{1}, s_{2}, \cdots, s_{a}}_{<p},\underbrace{p_{1}, p_{2}, \cdots p_{b}}_{=p},\underbrace{u_{1}, u_{2},\cdots, u_{c}}_{\text{not partitioned}},\underbrace{l_{1},l_{2},\cdots,l_{d}}_{>p}
$$
where $s$ represents the values that are smaller than the pivot value, $p$ represent the values that are equal to the pivot value, $u$ represents the values that are not partitioned, whereas $l$ represents the values that are larger than the pivot. 

### Denoting Partions

Now, let us denote the 4 segments in this list using indexes $i, j, k, l$, where

1. $[0, i)$ represents the partioned smaller part where each element in it is smaller than the pivot; 
2. $[i, j)$ represents the partioned pivot part where each element in it is equal to the pivot; 
3. $[j, k)$ represents the unsorted part;
4. $[k, l)$ represents the partitioned larger part where each element in it is larger than the pivot ($l$ represents the length of the list). 

### Invariants

Now, if we only look at the indexes, the list, at any stage of the partionting, shall look something like this:

$$
\underbrace{0,  \dots, i-1}\_{<p},\underbrace{ i, \cdots, j-1}\_{=p},\underbrace{ j, \cdots, k-1}\_{\text{not partitioned}}, \underbrace{k, \cdots, l}\_{>p}
$$

and the invariant shall always hold that

$$0\leq i \leq j \leq k < l$$

### Initial Condition

When we first begin, $i$ should be $0$, $j$ should be $1$, and $k$ should be $l$. Therefore:

1. the first segment, $[0, i) = [0, 0)$, is an empty list of length $0$, which corresponds to the fact that we have not put any number to the smaller partition yet;
2. the second segment, $[i, j)=[0,1)$, is a list of length $1$ that contains the first element, which is the pivot value. If we are selecting pivot points randomly, we shall swap it with the first element in the list, and now the $[0,1)$ sublist will contain the pivot;
3. the third segment, $[j, k) = [1, l)$, is the rest of the list, because we have not partitioned any portion of the list besides putting the pivot into the first element;
4. the last segment, $[k, l) = [l, l)$, is an empty list of length 0, which corresponds to the fact that we have not put any number to the larger partition yet.

### Partitioning Element

To begin our partition, we shall be looking at the value at index $j$, which represents the start of the unpartitioned portion. We shall compare it with the pivot value, and 3 situations may occur. 

1. if the value at index $j$ is smaller than the pivot value, we replace it with the value at $i$, which is the start of our pivot section, we then increment $i$ and $j$ both by $1$. Essentially, we are sliding the pivot section forward by 1.
2. if the value at index $j$ is larger than the pivot value, we replace it with the value at $k-1$, and we decrement $k$ by $1$.
3. if the value at index $j$ is equal to the pivot value, we increment $j$ by 1. Essentially, we are increasing the size of the pivot section by $1$.

### Termination Condition

Recall in the [Denoting Partions](#denoting-partions) section, we have divided the list into 4 segments, one of with is the unpartitioned segment, which starts at $j$ (inclusive) and ends at $k$. Therefore, if $j=k$, it would mean that the unpartitioned segment would have a length of $0$, which means that the entire list is partitioned. Therefore, we terminate the partitioning process. 

## Code Implementation

TBD