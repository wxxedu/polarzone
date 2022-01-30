---
title: "Selection Sort"
date: 2022-01-31T00:16:21+08:00
draft: false
series: ["CS2040S"]
tags: ["CS2040S", "Notes", "Data Structures", "Algorithms", "Sorting"]
---


## Idea

One way of sorting a list is that we iterate through the list, and for every iteration, we pick out the least item and move it to the front. 

Suppose below is the list that we want to sort. 

```
1 9 7 5 4 6 8 10 3 2
```

Firstly, we find out the least item in the entire list through iteration. In this case, the least item is $1$, which happens to be the first item of the list. Hence, we leave it there.

Then, we start from the second item and try to find out the least item until the end of the list. In this case, the least item is $2$, which is the tenth item. Hence, we swap the second item with the tenth item. By now, the list should look like this.

```
1 2 7 5 4 6 8 10 3 9
```

Then, for the third round, we find the least item between the third item and the last item (inclusive of both ends). In this case, it is 3 and is the nineth item. Hence, we swap the third item with the nineth item. The list should now look like this:

```
1 2 3 5 4 6 8 10 7 9
```

For the fourth round, the list should look like this:

```
1 2 3 4 5 6 8 10 7 9
```

And the we should go on until the entirety of the list is sorted. 

## Code

### Java Implementation

```java
class SelectionSort {
	public void sort(int[] nums) {
        int length = nums.length; 
        for (int i = 0; i < length; i ++) {
			// 1
            int minIdx = i; 
			// 2
            for (int j = i; j < length; j ++) {
                if (nums[j] < nums[minIdx]) {
                    minIdx = j;
                }
            }
			// 3
            int tmp = nums[i];
            nums[i] = nums[minIdx];
            nums[minIdx] = tmp;
        }
    }
}
```

1. we define a `minIdx` to denote the minimum value's index;
2. we loop through the list starting from the index i such that we won't be messing with the first i-1 elements, which are already sorted;
3. we swap the value at the `minIdx` and the `i` th value.
## Complexity Analysis

Note that in the code above, there are two loops nested in each other. The first loop runs $n$ times, with $n$ being the size of the loop. The second loop runs $n-i$ times. Hence, the total times that the code would be run is:

$$
\begin{align*}
&1+2+3+\cdots+n \\
=&\dfrac{n(n+1)}{2}=O(n^{2})
\end{align*}
$$

Note that for selection sort, $\Omega$ and $\Theta$ and $O$ are the same. I.e. they are all $n^{2}$ complexity. The reason for this is that for each loop, you'll have to loop through the entirety of the remaining list to find out the minimum value. No matter what the list looks like, this complexity will not be altered.