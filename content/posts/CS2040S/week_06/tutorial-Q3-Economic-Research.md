---
title: "Tutorial Q3 Economic Research"
date: 2022-02-16T21:56:05+08:00
draft: false
series: ["CS2040S"]
tags: ["CS2040S", "Notes", "Data Structures", "Algorithms"]
---

> You are an economist doing a research on the wealth of different generations in Singapore. You have a huge (anonymised) dataset that consists of ages and wealth, for example, it looks something like:
> ```
> 24    150,000
> 32    42,000
> 18    1,000
> 78    151,000
> 60    109,000
> ```
> That is, each row consists of a unique identifier, an age, and a number that represents their amount of wealth.
> Your goal is divide the dataset into “equi-wealth” age ranges. That is, given a parameter k, you should produce k different lists as output $A_1$, $A_2$, ..., $A_k$ with the following properties:
> 1. All the ages of people in set Aj should be less than or equal to the ages of people in $A_{j+1}$. That is, each set should be a subset of the original dataset containing a contiguous age range.
> 2. The sum of wealth in each set should be (roughly) the same (tolerating rounding errors if k does not divide the total wealth, or exact equality is not attainable).
> In the example above, if taking the first five rows and k = 3, you might output $(3,1)$,$(2,5)$,$(4),$ where the age ranges are $[0, 30)$, $[30, 70)$, $[70, ∞)$ respectively, with the same total wealth of $151, 000$. Notice this means that the age ranges are not (necessarily) of the same size. There are no other restrictions on the output list. You should assume that the given k is relatively small, e.g., $9$ or $10$, while the dataset is very large, e.g., the population of Singapore. Also note that the dataset is unsorted.
> Design the most eﬀicient algorithm you can to solve this problem, and analyse its time complexity.

## 1. Naive Solution

Notice that because we have the condition be such that the sum of wealth of each sub group should be the same, we can calculate the sum of wealth of each sub group by dividing the total sum by the number of groups, which is given by 

$$
\text{W}\_{\text{group}}=\dfrac{W\_{\text{total}}}{k}
$$

where $W$ represents wealth. Since we were to find $k$ groups, the naive solution would be to sort out the list by age ($O(n\log n)$), and then iterate through the list to put the items into their relative groups ($O(n)$). Therefore, the final complexity for this issue would be $O(n\log n)$. 

> For simplicity of reference later on, we shall call $W_{group}$ target, as it is the target sum value that we are looking for in each group.

## 2. Quickselect

The issue with the previous method is that we don't actually need to sort out every element in the list. Consider the first group, we don't need to have it internally sorted out by the age. Rather, we would only need to make sure that **the ages of people in it are all smaller than the ages of people in the groups following it**.
$$
x\_1, x\_2, \cdots, x\_{k} \leq p\_1 < x\_{k+1}, x\_{k+2}, \cdots,x\_{l} \leq p\_{2} < x\_{l+1}, \cdots x\_{m}
$$
In other words, suppose that we only need to divide the dataset into two groups, we will only make sure that we find a pivot point where the values(age) on the left side of the pivot are smaller than the values on the right side of the pivot. This shouldn't sound too strange -- it is the same mechanism that we use in quicksort and quickselect. However, instead of using quick sort to sort everything out, we will only need to use quicksort to find out the first group, which shall take $O(n)$ time. The way we do this is simple:

We use quickselect to find out the median value (of age). Because we have applied quickselect, the left part of list shall contain people whose age are smaller than the median age (represented by $s$ below:

$$
\underbrace{s, s, s, \cdots, s}\_{\text{first }\frac{n}{2}\text{ elements}}, \underbrace{m, l, l, l, \cdots, l}\_{\text{remaining }\frac{n}{2}\text{ elements}}
$$
Now, we shall find the sum of the wealth of the first half of the list. 

If the total wealth of the first half of the list is smaller than the target value, it means that the first group should have more than $n/2$ elements. In other words, we should be selecting more elements from the second half of the array and put them into our goup. Practically, we shall set the target (see the section above) to the target minus the sum of wealth of the first $n/2$ elements. Then, we shall find out the median of age in the second half of the list using quick select.

Else, it means that the first group should have less than $n/2$ elements. If it has less than $n/2$ elements, we shall consider the first $n/2$ elements and find out the median age in that sublist, consequently partioning the first $n/2$ elements into two parts, each of size $n/4$, using quick select.

Eventually, we will find the pivot point where the sum of all the values to the left of the pivot is equal to the target value (see the section above for what the target means). Note that the time complexity for this whole process of finding out the first group is:

$$
O(n)+O(\frac{n}{2})+O(\frac{n}{4})+\cdots O(1)=O(n)
$$

Therefore, the time complexity for finding the smallest group is $O(n)$. Since we are going to divide the list into $k$ groups, where $k$ will mostly be a small number relative to $n$, the time complexity for this solution is going to be $O(kn)$, which is smaller than the $O(n\log n)$ solution by just sorting out the array. 

## $O(n\log k)$ Solution

The algorithm can be futher optimized to $O(n\log k)$.  

To achieve this, we divide the algorithm into three steps.

### Step 01 - Dividing Into Buckets ($O(n\log k)$)

We shall use the quick select to divide the array into $k$ segments. This would require $\log k$ levels of recursion. Because for each level of recursion, it costs $O(n)$, we would have an overal time complexity of $O(n\log k)$.

### Step 02 - Finding the Buckets with Pivots ($O(n)$)

Because we know the target value, we can go through go though the list, find out the sum of each small section, and decide whether if our target of sum of wealth lies in that bucket. For example, if we have a target of wealth of 20, and if the sum of the first bucket is 10, then it means that it is not in the first bucket. If it is 25, then it is in the first bucket. If it is 45, then we can find 2 groups inside the first segment. 

### Step 03 - Applying the Quickselect solution

We apply the solution that we discussed in the second section, which would take $O(n)$ to find out the first few elements that adds up to the target value. Because we are only considering the small section which has the length $n/k$, the time complexity for finding out the elements whose wealth add up to the target value in a single bucket costs $O(n/k)$. Because we have $k$ buckets, the resulting time complexity is $O(n)$.

### Conclusion

Because we have three steps, the first step takes $O(n\log k)$, the second step takes $O(n)$, and the third step takes $O(n)$, the resulting time complexity should be $O(n)$.