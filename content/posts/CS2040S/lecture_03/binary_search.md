---
title: "CS2040S Lecture 03 Notes - Binary Search"
date: 2022-01-18T17:34:18+08:00
draft: false
series: ["CS2040S"]
tags: ["CS2040S", "Notes", "Data Structures", "Algorithms", "Binary Search"]
---

Module: [CS2040S](../..)

Lecture: [Lecture 03](..)

> Binary search is **HARD**!

## What is Binary Search?

Bineary search is an algorithm that allows you to find the item you want in a **sorted** list in $O(\log(n))$ time. It accomplishes this by dividing the list into two parts, check which part the target value is in, and then go on to divide that part of the list into two parts ......

## An Example by Hand

Here is an example where we do binary search by hand. Suppose we are given a list from $1$ to $10$, and our goal is to find $7$ in the list. 

We begin by dividing the list into two parts, which is $1$ to $5$ and $6$ to $10$.

$$
\begin{align*}
\text{List1} &= [1, 2, 3, 4, 5] \\
\text{List2} &= [6, 7, 8, 9, 10]
\end{align*}
$$

To find out which part $7$ is in, we simply compare $7$ with the middle value of the list, which in this case could be $5$ or $6$. *For the sake of convenience, we round down on the middle value (i.e. if there are two numbers in the middle, we take the first one).*

We compare $7$ with $5$, and know that $7>5$, hence we can be sure that **$7$ is in the second list**. Note that if on planet Mars $7\leq5$ is true, we would then know that $7$ is in the first list. **Unfortunately, that is not the case here**. 

So now, we shall look into the second list, which is $[6, 7, 8, 9, 10]$. Similarly, we would like to divide the list into two parts:

$$
\begin{align*}
\text{List2-1}&=[6,7,8]\\\\
\text{List2-2}&=[9,10]
\end{align*}
$$

Similarly, we would want to find out which list our missing $7$ is. Hence, we need to compare it to the value in the middle, which in this case, is $8$. As my kindergarten brother would tell you, $7$ is less than $8$, **so $7$ is in the first list**. 

Here, we then go on to explore the first list, i.e. $\text{List2-1}$:

$$
\begin{align*}
\text{List2-1-1}&=[6,7]\\\\
\text{List2-1-2}&=[8]
\end{align*}
$$

By the similar token, we compare $7$ to the middle value, which in this case, is $7$ itself! Hurray, we found $7$! But wait, we haven't finished yet. 

We should still abide by the rules that we set in the beginning: if $7$ is less than or equal to the middle value, we then go on to investigate the first list; if $7$ is greater than the middle value, we would go on to investigate the second list. Hence, because $7<=7$ is true, we should go on and explore the first list. 

$$
\begin{align*}
\text{List2-1-1-1}&=[6]\\\\
\text{List2-1-1-2}&=[7]
\end{align*}
$$

By the similar token, if we compare $7$ to the middle value, which, in this case, is $6$. Because $7>6$, we know immediately that $7$ is in the second list. Because there is only 1 value in the second list, we found $7$!

*Of course, in the scenario above, we could just end the program when we first find $7$, but frankly the influence on the running time of our program is so little that we could just ignore it and let the program run until it's done.*

## Writing Some Code

### Recursion??? NO!

So, how are we going to actually implement this in code? Intuitively, we could use **recursion** to divide up the lists just like what we did by hand. Below is an example in Java. 

```java
import java.util.*;

class BinarySearch {
	// finds out sub array from pos beg to pos end
	// need not care
	public static int[] subArray(int[] array, int beg, int end) {
		 return Arrays.copyOfRange(array, beg, end + 1);
	}

	public static int recursiveFind(int[] lst, int key) {
		int begin = 0;
		int end = lst.length - 1;
		int middle = end / 2; // 1
		
		// 2
		if (end == 0) {
			return 1;
		}
		
		// recursive steps
		if (key > lst[middle]) {
			// 3
			int[] secondLst = subArray(lst, middle + 1, end);
			return middle + 1 + recursiveFind(secondLst, key); // 4
		} else {
			// 5
			int[] firstLst = subArray(lst, 0, middle);
			return recursiveFind(firstLst, key);
		}
	}
}
```

1. Note that Java automatically rounds down when we use `/` on integers.

2. The termination condition: similar to when we manually did the binary search, if the length of the list is $1$, or in this case when the end postion is $0$, we terminate.

3. If the `key` is greater than the middle value (`lst[middle]`), then we should be looking at the second part of the list.
4. Because we want to find the indexes, we would need to add the number of elements before to the result of the recusive call on the second half of the list. In this case, it is `middle + 1`.

5. If the `key` is smaller than the middle value (`lst[middle]`), then we should be looking at the first part of the list. 

> Well, what are the problems with using recursions, then?

To begin with, while using recursions, we started manipulating arrays, i.e. finding the **subarray** of the list. It turns out that this operation is **really inefficient**! The copying action would itself cost $O(n)$, which demolishes all the advantages of using binary search!

Secondly (but not as important), it is just such a mess writing the code for the recursion using recursion. Because in each recursive call, we could only view the given portion of the list, we have to calculate the index at the given point. It may lead to a lot of potential mistakes.

### Iteration? Yes!

How should we deal with binary search in code, then? Use iteration. The idea is that we don't have to change the list -- **we can just use two indexes, begin and end, to signify the portion of the list that we are investigating.** 

In the very beginning, we set `begin` to index `0`, and `end` to the length of the input array minus 1 (`lst.length -1`) (because the indexes starts at $0$!). Then, we compare the middle value with the `key`. If the `key` is greater than the middle value, we set `begin` to the middle index plus 1; else, we set the end to the middle index. We repeat this process until the `begin` is equal to the `end`. At this point, the `begin` should be the index that we are looking for. 

```java
class BinarySearch {
	public static int iterativeFind(int[] lst, int key) {
		int begin = 0;
		int end = lst.length - 1;
		while (begin != end) {
			int middle = begin + (end - begin) / 2; // 1
			if (key > lst[middle]) { // 2
				begin = middle + 1; // 3
			} else {
				end = middle;
			}
		}
		return end;
	}
}
```

1. Note that java automatically rounds down when using division on integers. It should also be noted that we use `begin + (end-begin)/2` instead of `(begin + end)/2` to avoid integer overflow errors. (*It's rare, but we have to be cautious!*)

2. If `key` is greater than the value at `middle`, it means that `key` is in the second part of the array. Hence, we set `begin` to `middle + 1`.

3. Because we rounded down when calculating `middle`, it means that `middle` will always represent the index of the last item in the first part of the array. Hence, we need to add 1.

4. If `key` is less than the value at `middle`, it means that `key` is in the first part of the array. Hence, we set the end to `middle`.

## Disclaimer

I titled this series CS2040S because it is the notes that I wrote while taking the [CS2040S Data Structures and Algorithms](https://nusmods.com/modules/CS2040S/data-structures-and-algorithms) at NUS. However, I do not by any means represent the teaching team at NUS (I am only a newbie student taking this course), nor do the contents that I wrote represent what was taught in class. 

I wrote these notes and published them on my blog because I am a strong believer in the [Feymann Method](https://en.wikipedia.org/wiki/Learning_by_teaching). I do believe that by writing down my understanding of the course content could help me learn better. Hence, the only relevance to the CS2040S course is that I would try to explain the topics covered in the course using my own language. While writing, I would try my best to share with you my thought processes while learning these contents. 

It should also be reminded that, because I am a newbie to algorithms and data structures, I cannot guarentee that what I wrote are 100% correct and accurate. If you were to refer to my own blog, please use it at your own discretion. It would be hugely appreciated if you could [email](mailto:xiuxuan.wang@u.nus.edu) me the mistakes if you happen to find some. 