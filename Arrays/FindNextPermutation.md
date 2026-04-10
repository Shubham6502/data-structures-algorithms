# Next Permutation in Java

## 📌 Problem Statement
Given an array of integers `nums`, rearrange the numbers into the **next lexicographically greater permutation**.

If such an arrangement is not possible, rearrange it as the **lowest possible order** (i.e., sorted in ascending order).

The replacement must be done **in-place** using only constant extra memory.

---

## 🚀 Approach

The algorithm follows these steps:

1. **Find Pivot**
   - Traverse from right to left.
   - Find the first index `i` such that:
     ```
     nums[i] < nums[i + 1]
     ```
   - This index is called the **pivot**.

2. **If Pivot Not Found**
   - The array is in descending order.
   - Reverse the entire array to get the smallest permutation.

3. **Find Successor**
   - Traverse from the end.
   - Find the first element greater than `nums[pivot]`.
   - Swap it with `nums[pivot]`.

4. **Reverse the Suffix**
   - Reverse elements from `pivot + 1` to the end.

---

## 🧠 Time and Space Complexity

- **Time Complexity:** `O(n)`
- **Space Complexity:** `O(1)` (in-place)

---

## 💻 Code Implementation

```java
class Solution {

    public void reverse(int[] nums, int start, int end) {
        while (start < end) {
            int temp = nums[start];
            nums[start] = nums[end];
            nums[end] = temp;
            start++;
            end--;
        }
    }

    public void nextPermutation(int[] nums) {
        int pivot = -1;
        int n = nums.length;

        // Step 1: Find pivot
        for (int i = n - 2; i >= 0; i--) {
            if (nums[i] < nums[i + 1]) {
                pivot = i;
                break;
            }
        }

        // Step 2: If no pivot, reverse whole array
        if (pivot == -1) {
            reverse(nums, 0, n - 1);
            return;
        }

        // Step 3: Find next greater element and swap
        for (int i = n - 1; i > pivot; i--) {
            if (nums[i] > nums[pivot]) {
                int temp = nums[i];
                nums[i] = nums[pivot];
                nums[pivot] = temp;
                break;
            }
        }

        // Step 4: Reverse the suffix
        reverse(nums, pivot + 1, n - 1);
    }
}
