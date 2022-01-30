---
title: "Insertion Sort"
date: 2022-01-31T00:15:22+08:00
draft: false
series: ["CS2040S"]
tags: ["CS2040S", "Notes", "Data Structures", "Algorithms", "Sorting"]
---

In [merge sort](../merge_sort), our goal was to make sure that the first $i$ items are sorted and are the least items. However, the process of finding the least item in each iteration is actually quite expensive - you'll have to iterate through the whole least. Hence, an alternative idea would be to simply make sure that the first $i$ items are sorted, and nothing more. Therefore, we do not need to search for the least item. Instead, we would only need to find a place to insert the next item in the first $i$ items. 

For example, here is a least that we find:

```
1 9 7 5 4 6 8 10 3 2
```

Suppose we are going to have the items sorted, our goal is to iterate through the list, and make sure that the first $i$ items are sorted. 

For first round, we look at the first 1 item. Obviously, it is sorted. So we go on, and we look at the first 2 items. $9>1$, hence the first 2 items are also clearly sorted. Then, we look at the first 3 items. We realize that they are not sorted, so we need to take a look at where to place the third item. We put it in the second place, i.e. replace it with 9. Here, we obtain the list:

```
1 7 9 5 4 6 8 10 3 2
```

Now, if we go on and take a look at the next item, we realize that 5 is also not sorted. Hence, we insert it into the first 4 items, and obtain:

```
1 5 7 9 4 6 8 10 3 2
``````

We repeat this process on and on and on, and eventually we would get a sorted list. 

## Code

### Java Implementation

```java
class InsertionSort {
    public void sort(int[] nums) {
        int length = nums.length;
        // 1
        if (length <= 1) {
            return;
        }
		// 2
        for (int i = 0; i < length - 1; i ++) {
			// 3
            if (nums[i+1] > nums[i]) {
                continue;
            }
			// 4
            for (int j = i; j >= 0; j --) {
				// 5
                if (nums[j+1] < nums[j]) {
                    int tmp = nums[j+1];
                    nums[j+1] = nums[j];
                    nums[j] = tmp;
                } else {
					// 6
                    break;
                }
            }
        }
    }
}
```

### C Implementation

```c
void insertionSort(int nums[], int length) {
    // 1
    if (length <= 1) {
        return;
    }
	// 2
    for (int i = 0; i < length -1; i ++) {
		// 3
        if (nums[i+1] > nums[i]) {
            continue;
		}
		// 4
        for (int j = i; j >= 0; j--) {
			// 5
            if (nums[j+1] < nums[j]) {
                int tmp = nums[j];
                nums[j] = nums[j+1];
                nums[j+1] = tmp;
            } else {
				// 6
				break
			}
        }
    }
}

```

(I kinda want to practice writing in `C`, so I wrote the `C` program. I was shocked that the `C` implementation looked almost exactly the same as the `java` one. LMAO)

1. Checks if the length is less than 1. In this case, we don't need to do anything.
2. Loop through the list. In each iteration, we consider the first $i$ items to be sorted, and the $(i+1)^{\text{th}}$  item to be the one to be inserted into the previous items. 
3. Because we assume the first $i$ elements are sorted, if the $(i+1)^{\text{th}}$ item is larger than the $i^{\text{th}}$ item, then the first $i+1$ items are sorted.
4. We start from the $i^{\text{th}}$ item backward to the first item.
5. If the current item and the item next to the current item are in the wrong order, we replace them.
6. If the order is correct, we know that we've put the item in the correct place. Hence, we would no longer need to do the search.

## Time Complexity

The time complexity of the sorting is $O(n^{2})$. The tighter bound complexity is also $\Theta(n^{2})$. However, the lowerbound of the insertion sort is only $O(n)$ - if you are given an already sorted array, it would only take $O(n)$ time to sort the array. 