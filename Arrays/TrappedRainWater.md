# Trapping Rain Water - Java Solution

## 📌 Problem Statement
Given an array `height[]` representing the elevation map where the width of each bar is 1, compute how much water it can trap after raining.

---

## 💡 Approach (Prefix & Suffix Arrays)

To solve this problem efficiently, we use two auxiliary arrays:

- **leftMax[]** → stores the maximum height to the left of each index
- **rightMax[]** → stores the maximum height to the right of each index

### Steps:
1. Traverse from left to right to fill `leftMax[]`
2. Traverse from right to left to fill `rightMax[]`
3. For each index, calculate trapped water:

water = min(leftMax[i], rightMax[i]) - height[i]

4. Sum all trapped water

---

## 🚀 Code Implementation

```java
class Solution {
 public int trap(int[] height) {
     int n = height.length;
     int leftMax[] = new int[n];
     int rightMax[] = new int[n];

     leftMax[0] = height[0];
     for(int i = 1; i < n; i++){
         leftMax[i] = Math.max(leftMax[i - 1], height[i]);
     }

     rightMax[n - 1] = height[n - 1];
     for(int i = n - 2; i >= 0; i--){
         rightMax[i] = Math.max(rightMax[i + 1], height[i]);
     }

     int trappedWater = 0;
     for(int i = 0; i < n; i++){
         trappedWater += (Math.min(rightMax[i], leftMax[i]) - height[i]);
     }

     return trappedWater;
 }
}
```
📊 Example  
Input:  
height = [0,1,0,2,1,0,1,3,2,1,2,1]  
Output:
6  


⏱️ Complexity Analysis  
Time Complexity: O(n)  
Space Complexity: O(n)  
