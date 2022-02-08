---
title: "Quick Sort"
date: 2022-02-07T23:16:11+08:00
draft: false
series: ["CS2040S"]
tags: ["CS2040S", "Notes", "Data Structures", "Algorithms", "Sorting"]
---

## 1. Idea Walkthrough

### 1.1. Idea of Quick Sort

The idea of Quick Sort is that we divide a list into three parts, the middle part consists of elements of the same value, which we call pivots, the left part consists of elements that have smaller value than the pivots, and the right part consists of elements that have larger value than the pivots. 

Quick Sort can be described in the following procedure:

1. find a pivot point number
2. for each pivot point number, iterate through the list such that numbers smaller than the pivot number is on the left of the pivot number, and numbers bigger than the pivot number is on the right of the pivot number
3. for the left half and the right half, recurse this process

### 1.2. Sorting a List

Suppose that we have a list 

```
1 9 7 5 4 6 8 10 3 2
```

Now, our goal is to sort this list using quick sort manually. 

#### 1.2.1. Find a Pivot Number

Firstly, we pick a pivot number. For this part, we shall pick the number in the middle of the list as the pivot. You will see later why this is not a good solution, and what number we should pick instead. 

#### 1.2.2. Partitioning the List

After picking the pivot number, we do the partition part where we put numbers smaller than the pivot before the pivot, and the numbers larger than the pivot after the pivot. 

Here is how this is done. We shall use two indexes, one from the start, and one from the end. 

- We first put the pivot at the start index. 
- Then, we look at the value at start index + 1. If the value is less than the pivot, we move forward the start index until a point where start index + 1 is greater than the pivot. 
- At that point, we exchange the value at the start index with the pivot, and stop. 
- Then, we start from the end and move the end index backwards if the value at the end index is greater than the pivot. If it is less than the pivot, then we stop. 
- After we stop, we swap the number at start index + 1 and end index. 

In this case, our pivot is 4. 

```
1 9 7 5 4 6 8 10 3 2
s                  e
```

The first thing that we do is that we put 4 to the start index, s. We do so by exchanging the value at s with 4. After the operation, the list should look like this:

```
4 9 7 5 1 6 8 10 3 2
s                  e
```

We look at the index s+1. the number at s+1 is 9, which is greater than the pivot. So we stop here without traversing forward. 

```
4  9  7 5 1 6 8 10 3 2
s s+1                e
```

Then, we shall look at the end index. If the number at the end index is greater than 4, then we step backward. Else, we stop there. In this case, $2 < 4$, so we stop at the end. 

Now, after finishing at both ends, we exchange the values between s+1 and e, giving us:

```
4  2  7 5 1 6 8 10 3 9
s s+1                e
```

After all of this is done, we shall move exchange the pivot with the value at s+1, and add 1 to s, and at the same time move e forward, giving us:

```
2 4  7  5 1 6 8 10 3 9
  s s+1            e
```

Then we repeat the process described above, and we shall find: 

```
2 3 4  5  1 6 8 10 7 9
    s s+1          e
```

Then we repeat this process again, and we shall have:

```
2 3 1 4 5 6 8 10 7 9
      s e
```

#### 1.2.3. Recursion

Now that we have successly permutated the list such that the portion on the left of 4 is less than 4, while the portion on the right of 4 is greater than 4. Hence, we end up with two small lists. With this, we shall be use recursion to sort out the rest. 

## 2. Best Case Time Complexity 

Now, assume that we have the best scenario here. Suppose that every time when we pick something, we find the exact middle point of the sublist. Then, the sorting time complexity would look something like this:

![Quick Sort Best Case Time Complexity Diagram](/static/CS2040S/quick-sort-best-case.svg)

We realize that the time complexity here would be $n\log n$, because the tree is of depth $\log n$ and for each iteration it would take $n$ total iterations to conduct the partition. Therefore, we shall find the best case time-complexity of the Quick Sort algorithm is $\Omega(n\log n)$.

## 3. Pivot Points

From the discussion of the time complexity above, we realize one thing: *the efficiency of the Quick Sort algorithm depends on the pivot point picked*. 

### 3.1. Naively Picking a Pivot Point

To further convey this idea, if we just naively choose a pivot point, say, always picking the first element as the pivot point, then we would encounter huge problems with the Quick Sort. Suppose that we were to deal with the following list of numbers:

```
10 9 8 7 6 5 4 3 2 1
```

Here, for the first iteration, we pick 10 as the pivot point, then we would end up with:

```
9 8 7 6 5 4 3 2 1 10
```

After this, if we would pick 9 as the pivot point, which would result in 

```
8 7 6 5 4 3 2 1 9 10
```

so on and so forth, it would take us n iterations to finish sorting the list. Therefore, the time complexity of this thing would be $O(n^{2})$. This is EXTREMELY SLOW. But wait, isn't this thing called Quick Sort? 

Well, in most cases, the Quick Sort algorithm would have good performance. So, technically, it is still "quick." But to solve issues where the Quick Sort would take an astronomically long time, we could actually **pick pivot points at random**. 

### 3.2. Randomized Algorithm

Here comes the **randomized algorithm**! 

Instead of assigning a fixed way of picking a pivot point, we can pick a pivot point at random. This way, the algorithm would surely be run at $O(n\log n)$ time. 

To fully illustrate this point, when we randomly pick a number from the list, we assume that it is in the middle 50\%, i.e. if the list is sorted, the item would be between 25\% to the 75\% of the list. 

Now, to make sure that this situation happens, we add the following procedure in the list. When we partition the list, if we find out that the pivot that we picked is not in the middle 50\% of the list, we shall redo the partitioning until this condition if satisfied. Note that this is not necessary at all for writing the Quick Sort algorithm, and we make this modication for ease of analysis. 

Now, if we were to make sure that the pivot was in the middle 50\%, on average, it would take us 2 tries to get the correct pivot point. 

Assume that we are getting the worst pivot in this case each time, i.e. for each time we are getting the pivot that is exactly the 75\% if we ranked the numbers from small to large, then we would have the following tree:

![Paranoid Quick Sort Analysis](/static/CS2040S/paranoid-quick-sort-time-complexity.svg)

Now, if we take a look at this tree. On every level, we are having a total of $n$ operations. The problem here is to figure out how many levels are there.

To figure that out, we should be focusing on the longest path, which, in this case, is the following path marked in green:

![Paranoid Quick Sort Analysis (Highlighted)](/static/CS2040S/paranoid-quick-sort-time-complexity-highlighted.svg)

we notice that on each iteration, the number of elements becomes $3/4$ of its parent. Therefore, if we start from the bottom of the tree, which has 1 element, and we go up, we would each time increase the element number by $4/3$. Hence, we have the function $(4/3)^{t}=n$, where $t$ is the number of levels that we are interested to know. Expressed in logarithmic terms, we have:

$$t=\log_{\frac{4}{3}}n$$

Because we know that the bases in logarithmic functions don't realy matter ($\log_{a}b=(\log_{2}b/\log_{2}a)$, $\log_{2}a$ is a constant), we shall know that the order of growth of the number of levels is $O(\log n)$. Hence, the time complexity for the algorithm is $O(n\log n)$.

## 4. Duplicates

Now it seems all good. But in the cases discussed above, we have not handled duplicates. What should we do with duplicates?

### 4.1 Doing Nothing with Duplicates

In the steps described above, we have two indexes, the start index s and the end index e. In previous sections, we set the partition algorithm be such that if the number at s + 1 is less than pivot, we move s forward, and if the number at e is greater than pivot, we move e forward. **This will partition the array into 3 parts, the left part includes the numbers that are less than the pivot, the middle part is the pivot, and the right part includes the numbers that are greater than the pivot.**

One way of handling the duplicates is to modify this partition rule be such that we include the equal sign in either or both operations. **This way, depending on the situation, we could have numbers that are equal to the pivot be in either or both of the left and right part.** Suppose that we modify the partition algorithm be such that if the number at s+1 is less than or equal to the pivot, we move s forward, and if the number at e is greater than or equal to the pivot, we move e backward. 

This way, we may end up with a partitioned array like this (if our pivot is 5):

```plaintext
1 2 5 3 4 5 7 9 5 10 5
          ^
        pivot
```

Therefore, if we look at the left-half of the array, we realize that all the numbers in the left-half of the array are less than or equal to the pivot. If we look at the right-half of the array, we realize that all the numbers in the right half of the array are greater than or equal to the array. 

Now, what is the issue with this approach? Consider this very extreme case:

```plaintext
5 5 5 5 5 5 5 5 5 5
```

If we were to just ignore the pivot, the start index would end up in meeting the end index at the last element after the first partition:

```plaintext
5 5 5 5 5 5 5 5 5 5
                s e
```

then, for the left partition of the list, this situation would happen again:

```
5 5 5 5 5 5 5 5 5
              s e
```

this situation would go on and on and on, until we reach the first two elements:

```
5 5
s e
```

the result of this algorithm? $O(n^{2})$. We'll never want this happen!

### 4.2 Keeping the Pivots to a Separate Section

What shall we do to prevent the happening of this situation then?

Well, we could partition the array into three parts. The left part contains the numbers that are strictly smaller than the pivot, while the right part contains the numbers that are strictly larger than the pivot. The key here is the middle part. Instead of containing only one pivot point, we could modify it be such that it would contain all the numbers that is equal to the pivot. This way, we will no longer have to worry about the duplicated pivots!

So, the way that this is implemented is by adding indexes to keep track of the pivots. Say that p represents the pivot number, s represents numbers that are smaller than the pivot, and l represents numbers that are larger than the pivot:

```plaintext
s s s p p p l s l p l l s p p l l l l
    ^     ^                   ^
    a     b                   c
```

in the list above, 
- the index a represents the end of the end of the finished part of the smaller partition;
- b represents the end of the finished part of the pivot partition;
- c represents the begining of the finished part of the larger partition;
- the part between b and c are the elements yet to be sorted. 

## 5. Code Implementation

**[To Be Completed]**
