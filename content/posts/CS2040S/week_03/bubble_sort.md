---
title: "Bubble Sort"
date: 2022-01-31T00:16:14+08:00
draft: false
series: ["CS2040S"]
tags: ["CS2040S", "Notes", "Data Structures", "Algorithms", "Sorting"]
---

Suppose that we are given the following list:

```
1 9 7 5 4 6 8 10 3 2
```

The idea of bubble sort is that we iterate through the entire list several times, and for each time, we swap the adjacent values if their relative position is not correct. 

So, for the first iteration, we start looking at 1 and 9, and find out that their relative position is correct. We then go on to look at 9 and 7, and find that their relative position is incorrect. Hence, we would swap 9 and 7. The list would look like this:

```
1 7 9 5 4 6 8 10 3 2
```

we then go on to look at 9 and 5. Their relative position is also incorrect. Hence, we exchange them. The list would now look like this:

```
1 5 7 9 4 6 8 10 3 2
```

and so on. Hence, after the first iteration, the list would look something like this:

```
1 5 7 4 6 8 9 3 2 10
```

After the second iteration, the list would look like this:

```
1 5 4 6 7 8 3 2 9 10
```

So on and so forth, until the list is sorted.

## Code

### Java

```java
public class BubbleSort {
    public void sort(int[] nums) {
        int length = nums.length;
		// 1
        for (int i = 0; i < length; i ++) {
			// 2
            boolean hasSwapped = false;
			// 3
            for (int j = 0; j < length - i - 1; j ++) {
				// 4
                if (nums[j] > nums[j+1]) {
                    int tmp = nums[j];
                    nums[j] = nums[j+1];
                    nums[j+1] = tmp;
                    hasSwapped = true;
                }
            }
			// 5
            if (!hasSwapped) {
                return;
            }
        }
    }
}
```

### C

```c
void bubbleSort(int nums[], int length) {
	// 1
    for (int i = 0; i < length; i ++) {
		// 2
        bool hasSwapped = false;
		// 3
        for (int j = 0; j < length - i - 1; j ++) {
			// 4
            if (nums[j] > nums[j+1]) {
                int tmp = nums[j];
                nums[j] = nums[j+1];
                nums[j+1] = tmp;
                hasSwapped = true;
            }
        }
		// 5
        if (!hasSwapped) {
            break;
        }
    }
}
```

### Python

```python
def bubble_sort(lst):
    length = len(lst)
	// 1
    for i in range(length):
		// 2
        hasSwapped = False
		// 3
        for j in range(length - i - 1):
			// 4
            if lst[j] > lst[j + 1]:
                lst[j], lst[j + 1] = lst[j + 1], lst[j]
                hasSwapped = True
		// 5
        if not hasSwapped:
            break
    return lst
```

1. Because in each iteration, the last item would be put in place. Hence, we would at most iterate the length of the list times before the list is sorted. 
2. We want to take note if the list is already sorted. If so, we can just stop the iteration.
3. Because after each iteration, we can make sure that the last $i$ items are sorted, we would only need to iterate through the first $n-i$ items. 
4. Swap the two adjacent items if they are not in the correct relative position.
5. If we found out that after one iteration, we have not swapped any numbers, it means that the entire list is sorted. 

## Time Complexity

The lower bound of the time complexity is $\Omega(n)$. This is because if you were to have a sorted list, then it only needs to go through the list once to know that the list is sorted, and it would stop.

The upper bound of the time complexity is $O(n^{2})$, because it had nested loops. The tighter bound of the time complexity is $\Theta(n^{2})$.