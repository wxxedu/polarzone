---
title: "Merge Sort"
date: 2022-01-31T00:03:37+08:00
draft: false
series: ["CS2040S"]
tags: ["CS2040S", "Notes", "Data Structures", "Algorithms", "Sorting"]
graph: true
---

Suppose we have a list 

```
1 9 7 5 4 6 8 10 3 2
```

and our goal is to sort the list. Merge sort's idea is to divide the list into small segments and recombine them. 


![Merge Sort](/static/CS2040S/merge_sort_diagram.png)

Here, we have the list sorted. 

## Code

### Java Implementation

```java
public class MergeSort{

    public void sort(int[] nums) {
        copy(nums, _sort(nums));;
    }

    static void copy(int[] arr1, int[] arr2) {
        assert arr1.length == arr2.length: "trying to copy two arrays that do not have the same size";
        for (int i = 0; i < arr1.length; i ++) {
            arr1[i] = arr2[i];
        }
    }

    static int[] merge(int[] arr1, int[] arr2) {
        int l1 = arr1.length;
        int l2 = arr2.length;
        int[] res = new int[l1 + l2];
        int i1 = 0;
        int i2 = 0;
        while (i1 < l1 || i2 < l2) {
            if (i1 < l1 && i2 < l2) {
                int v1 = arr1[i1];
                int v2 = arr2[i2];
                if (v1 < v2) {
                    res[i1 + i2] = v1;
                    i1 += 1;
                } else {
                    res[i1 + i2] = v2;
                    i2 += 1;
                }
            } else if (i1 < l1) {
                res[i1 + i2] = arr1[i1];
                i1 += 1;
            } else {
                res[i1 + i2] = arr2[i2];
                i2 += 1;
            }
        }
        return res;
    }

    static int[] subArr(int[] arr, int begin, int end) {
        int[] res = new int[end - begin];
        for (int i = begin; i < end; i ++) {
            res[i - begin] = arr[i];
        }
        return res;
    }
    
    private int[] _sort(int[] nums) {
        if (nums.length <= 1) {
            return nums;
        } else {
            int mid = nums.length / 2;
            return merge(
                _sort(subArr(nums, 0, mid)),
                _sort(subArr(nums, mid, nums.length))
            );
        }
    }
}

```

### C Implementation 

```c
void copyArr(int arr1[], int arr2[], int length) {
    for (int i = 0; i < length; i ++) {
        arr1[i] = arr2[i];
    }
}

int* merge(int lst1[], int len1, int lst2[], int len2) {
    int idx1 = 0;
    int idx2 = 0;
    int *res = malloc(sizeof(int) * (len1 + len2));
    while (idx1 < len1 || idx2 < len2) {
        int currIdx = idx1 + idx2;
        if (idx1 < len1 && idx2 < len2) {
            int v1 = lst1[idx1];
            int v2 = lst2[idx2];
            if (v1 < v2) {
                res[currIdx] = v1;
                idx1 ++;
            } else {
                res[currIdx] = v2;
                idx2 ++;
            }
        } else if (idx1 < len1) {
            res[currIdx] = lst1[idx1];
            idx1 ++;
        } else {
            res[currIdx] = lst2[idx2];
            idx2 ++;
        }
    }
    return res;
}

int* _mergeSort(int lst[], int length) {
    if (length <= 1) {
        return lst;
    } else {
        int mid = length / 2;
        return merge(
            _mergeSort(subArr(lst, 0, mid), mid),
            mid,
            _mergeSort(subArr(lst, mid, length), length - mid),
            length - mid
        );
    }
}

void mergeSort(int lst[], int length) {
    copyArr(
        lst,
        _mergeSort(lst, length),
        length
    );
}
```

### Python Implementation

```python
def merge_sort(lst):
    if len(lst) <= 1:
        return lst
    else:
        mid = len(lst) // 2
        return merge(merge_sort(lst[0:mid]), merge_sort(lst[mid:]))

def merge(left, right):
    li = 0
    ri = 0
    ll = len(left)
    rl = len(right)
    res = []
    while (li < ll or ri < rl):
        if (li < ll and ri < rl):
            lv = left[li]
            rv = right[ri]
            if lv < rv:
                res.append(lv)
                li += 1
            else:
                res.append(rv)
                ri += 1
        elif (li < ll):
            res.append(left[li])
            li += 1
        else:
            res.append(right[ri])
            ri += 1
    return res
```

## Complexity Analysis

Merge sort takes $O(n\log n)$ to sort the list. 