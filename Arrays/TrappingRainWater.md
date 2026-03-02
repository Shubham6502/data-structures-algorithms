# 🌧️ Trapping Rain Water | GeeksforGeeks

## 📌 Problem Statement

Given an array `arr[]` representing the height of bars where the width of each bar is `1`, compute how much water can be trapped after raining.

Each element represents elevation height, and water can be stored between taller bars.

---

## 🧠 Approach (Prefix & Suffix Maximum Method)

To calculate trapped water efficiently:

1. Find the **maximum height on the left** of every index.
2. Find the **maximum height on the right** of every index.
3. Water stored at index `i` is:
min(leftMax[i], rightMax[i]) - arr[i]


4. Sum the water stored at all indices.

---

## ⚙️ Algorithm Steps

- Create two arrays:
  - `leftMax[]` → stores highest bar from left
  - `rightMax[]` → stores highest bar from right
- Traverse left → fill `leftMax`
- Traverse right → fill `rightMax`
- Calculate trapped water using minimum boundary height.

---

## ✅ Java Solution

```java
class Solution {
    public int maxWater(int arr[]) {

        int n = arr.length;

        int leftMax[] = new int[n];
        int rightMax[] = new int[n];

        // Left maximum boundary
        leftMax[0] = arr[0];
        for(int i = 1; i < n; i++){
            leftMax[i] = Math.max(leftMax[i-1], arr[i]);
        }

        // Right maximum boundary
        rightMax[n-1] = arr[n-1];
        for(int i = n-2; i >= 0; i--){
            rightMax[i] = Math.max(rightMax[i+1], arr[i]);
        }

        // Calculate trapped water
        int trappedWater = 0;
        for(int i = 0; i < n; i++){
            trappedWater += Math.min(leftMax[i], rightMax[i]) - arr[i];
        }

        return trappedWater;
    }
}
```
⏱️ Complexity Analysis

Complexity	Value

Time Complexity	O(n)

Space Complexity	O(n)

📊 Example

Input

arr = [3,0,2,0,4]

Output :
7


💡 Key Concepts Used

Prefix Maximum Array
,Suffix Maximum Array
,Greedy Observation
,Array Traversal

🚀 Learning Outcome

This problem teaches how preprocessing arrays helps reduce brute-force complexity from O(n²) to O(n).

👨‍💻 Author

Shubham Patil

GitHub: https://github.com/Shubham6502
