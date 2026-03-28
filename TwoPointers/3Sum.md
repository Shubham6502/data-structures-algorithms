# 🧩 3Sum Problem

## 📌 Problem Statement

Given an integer array `nums`, return all the **unique triplets** `[nums[i], nums[j], nums[k]]` such that:

* `i ≠ j`, `i ≠ k`, `j ≠ k`
* `nums[i] + nums[j] + nums[k] == 0`

---

## 🧠 Example

```text
Input: nums = [-1, 0, 1, 2, -1, -4]

Output:
[
  [-1, -1, 2],
  [-1, 0, 1]
]
```

---

## 🚀 Approach (Two Pointer Technique)

### Step 1: Sort the array

* Helps in using two pointers
* Makes duplicate handling easier

### Step 2: Fix one element

* Iterate using index `x`

### Step 3: Use two pointers

* `y = x + 1` (left pointer)
* `z = n - 1` (right pointer)

### Step 4: Find sum

* If sum == 0 → store result
* If sum < 0 → move `y++`
* If sum > 0 → move `z--`

### Step 5: Skip duplicates

* Avoid repeating triplets

---

## 💻 Code (Java)

```java
import java.util.*;

class Solution {
    public List<List<Integer>> threeSum(int[] nums) {

        List<List<Integer>> ans = new ArrayList<>();
        if (nums.length < 3) return ans;

        Arrays.sort(nums);

        for (int x = 0; x < nums.length - 2; x++) {

            if (x > 0 && nums[x] == nums[x - 1]) continue;

            int y = x + 1;
            int z = nums.length - 1;

            while (y < z) {
                int sum = nums[x] + nums[y] + nums[z];

                if (sum == 0) {
                    ans.add(Arrays.asList(nums[x], nums[y], nums[z]));

                    while (y < z && nums[y] == nums[y + 1]) y++;
                    while (y < z && nums[z] == nums[z - 1]) z--;

                    y++;
                    z--;
                } 
                else if (sum < 0) {
                    y++;
                } 
                else {
                    z--;
                }
            }
        }

        return ans;
    }
}
```

---

## ⏱️ Complexity Analysis

* **Time Complexity:** `O(n²)`
* **Space Complexity:** `O(1)` (excluding output)


