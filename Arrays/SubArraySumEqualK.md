# Subarray Sum Equals K

## 📌 Problem Statement
Given an integer array `nums` and an integer `k`, return the total number of continuous subarrays whose sum equals to `k`.



## 💡 Approach (Prefix Sum + HashMap)

This solution uses the **Prefix Sum** technique along with a **HashMap** to efficiently count subarrays.

### 🔹 Key Idea:
- Maintain a running sum (`curr`)
- Check if `(curr - k)` exists in the map
- If it exists, it means a subarray with sum `k` is found



## ⚙️ Algorithm Steps

1. Initialize:
   - `curr = 0` → to store running sum
   - `count = 0` → to store result
   - `map` → to store prefix sum frequencies
   - Put `(0,1)` in map to handle edge case

2. Traverse the array:
   - Add current element to `curr`
   - Check if `(curr - k)` exists in map
   - Add its frequency to `count`
   - Update map with current sum

3. Return `count`



## 🧠 Code

```java
class Solution {
    public int subarraySum(int[] nums, int k) {
        int curr = 0;
        Map<Integer, Integer> map = new HashMap<>();
        int count = 0;

        map.put(0, 1);

        for (int i : nums) {
            curr += i;
            count += map.getOrDefault(curr - k, 0);
            map.put(curr, map.getOrDefault(curr, 0) + 1);
        }

        return count;
    }
}
```
⏱️ Complexity Analysis -
Time Complexity: O(n)

Space Complexity: O(n) -
HashMap stores prefix sums
