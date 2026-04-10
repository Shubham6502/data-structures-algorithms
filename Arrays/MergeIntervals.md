# Merge Intervals in Java

## 📌 Problem Statement

Given an array of intervals where `intervals[i] = [start, end]`, merge all overlapping intervals and return an array of the non-overlapping intervals that cover all the intervals.

---

## 🚀 Approach

The solution follows these steps:

1. **Sort the Intervals**
   - Sort intervals based on their starting time.

2. **Initialize Current Interval**
   - Take the first interval as the current interval.

3. **Traverse Intervals**
   - For each interval:
     - If it overlaps with the current interval:
       - Merge them by updating the end value.
     - Otherwise:
       - Add the current interval to the result list.
       - Move to the next interval.

4. **Add Last Interval**
   - Add the final interval to the result list.

---

## 🧠 Time and Space Complexity

- **Time Complexity:** `O(n log n)` (due to sorting)
- **Space Complexity:** `O(n)` (for storing result)

---

## 💻 Code Implementation

```java
class Solution {
    public int[][] merge(int[][] intervals) {
        ArrayList<int[]> res = new ArrayList<>();

        // Step 1: Sort intervals based on start time
        Arrays.sort(intervals, (a, b) -> a[0] - b[0]);

        int curr[] = intervals[0];

        // Step 2: Traverse and merge
        for (int i = 1; i < intervals.length; i++) {

            // If overlapping, merge intervals
            if (curr[1] >= intervals[i][0]) {
                curr[1] = Math.max(curr[1], intervals[i][1]);
            } else {
                // No overlap, add current interval
                res.add(curr);
                curr = intervals[i];
            }
        }

        // Step 3: Add last interval
        res.add(curr);

        return res.toArray(new int[res.size()][]);
    }
}
