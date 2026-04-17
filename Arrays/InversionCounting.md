# Count Inversions using Merge Sort (HackerRank)

## 📌 Problem Statement
Given an array of integers, count the number of **inversions**.

An inversion is defined as a pair `(i, j)` such that:
- `i < j`
- `arr[i] > arr[j]`


## 🚀 Approach
We use a modified **Merge Sort** algorithm to efficiently count inversions.

### Key Idea:
- While merging two sorted halves:
  - If `arr[i] <= arr[j]`, no inversion.
  - If `arr[i] > arr[j]`, then all elements from `i` to `mid` form inversions.

👉 Number of inversions = `mid - i + 1`


## ⏱️ Time Complexity
- **O(n log n)**

## 💾 Space Complexity
- **O(n)** (temporary array used during merge)



## 🧾 Code (Java)

```java
import java.util.*;

public class Solution {

    public static long merge(int start, int mid, int end, List<Integer> arr) {
        int i = start;
        int j = mid + 1;
        List<Integer> temp = new ArrayList<>();
        long count = 0;

        while (i <= mid && j <= end) {
            if (arr.get(i) <= arr.get(j)) {
                temp.add(arr.get(i));
                i++;
            } else {
                count += (mid - i + 1);
                temp.add(arr.get(j));
                j++;
            }
        }

        while (i <= mid) {
            temp.add(arr.get(i));
            i++;
        }

        while (j <= end) {
            temp.add(arr.get(j));
            j++;
        }

        for (int k = 0; k < temp.size(); k++) {
            arr.set(start + k, temp.get(k));
        }

        return count;
    }

    public static long mergeSort(int start, int end, List<Integer> arr) {
        if (start >= end) return 0;

        int mid = (start + end) / 2;

        long left = mergeSort(start, mid, arr);
        long right = mergeSort(mid + 1, end, arr);
        long mergeCount = merge(start, mid, end, arr);

        return left + right + mergeCount;
    }

    public static long countInversions(List<Integer> arr) {
        return mergeSort(0, arr.size() - 1, arr);
    }
}
