# 🔢 Two Sum

## 📌 Problem Statement

Given an array of integers `nums` and an integer `target`, return the **indices** of the two numbers such that they add up to the target.

You may assume:

* Each input has **exactly one solution**
* You may not use the same element twice
* The answer can be returned in any order

---

## 🧾 Example

**Input**

```
nums = [2,7,11,15]
target = 9
```

**Output**

```
[0,1]
```

**Explanation**

```
nums[0] + nums[1] = 2 + 7 = 9
```

---

## 🧠 Approach (HashMap Optimization)

### Idea

Instead of checking every pair (which takes O(n²) time), we store previously seen numbers in a **HashMap**.

For each element:

1. Calculate the required value:

   ```
   complement = target - current number
   ```
2. Check if this complement already exists in the map.
3. If yes → solution found.
4. Otherwise, store current number with its index.

---

### Why This Works

The HashMap allows constant-time lookup, so we avoid nested loops and significantly improve performance.

---

## ⚙️ Algorithm Steps

1. Create an empty HashMap.
2. Traverse the array once.
3. For each element:

   * Compute remaining value needed.
   * Check if it exists in map.
   * If found → return indices.
   * Otherwise store current value and index.
4. Return empty array if no solution exists.

---

## 💻 Java Solution

```java
class Solution { 
    public int[] twoSum(int[] nums, int target) {
        HashMap<Integer, Integer> map = new HashMap<>();

        for (int i = 0; i < nums.length; i++) {
            int newTarget = target - nums[i];

            if (map.containsKey(newTarget)) {
                return new int[] { map.get(newTarget), i };
            }

            map.put(nums[i], i);
        }

        return new int[] {};
    }
}
```

---

## ⏱ Complexity Analysis

| Complexity Type  | Value    |
| ---------------- | -------- |
| Time Complexity  | **O(n)** |
| Space Complexity | **O(n)** |

### Explanation

* We iterate through the array once → O(n)
* HashMap operations take O(1) average time.

---

## 🔍 Key Concepts Used

* HashMap
* One-pass traversal
* Complement search technique
* Space–time optimization

---

## 🧩 Pattern Recognition

This problem introduces an important interview pattern:

✅ **Hashing for Fast Lookup**

Many problems use this same idea:

* Two Sum variations
* Subarray Sum Equals K
* Longest Consecutive Sequence

---

## 📚 Learning Takeaway

> When a problem asks to find pairs quickly, think about using a HashMap to trade space for speed.

---

## 🏷 Tags

`Array` `HashMap` `Easy` `Interview Favorite`

---

👨‍💻 **Author:** Shubham Patil
📈 Daily DSA Practice Journey
