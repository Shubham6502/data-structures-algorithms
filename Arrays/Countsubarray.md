# Subarrays With First Element Minimum | GeeksforGeeks

## Problem Statement

Given an array `arr[]`, count the number of **subarrays where the first element is the minimum element of that subarray**.

A subarray `[i...j]` is valid if:

arr[i] ≤ arr[k] for every k in the range [i, j]

---

##  Approach (Next Smaller Element + Monotonic Stack)

A brute force approach would check every possible subarray, leading to **O(n²)** complexity.

To optimize, we use a **Monotonic Stack** to efficiently find the **next smaller element to the right**.

For each index `i`:

- Find the **next index `j` such that arr[j] < arr[i]**
- All subarrays starting at `i` and ending before `j` will satisfy the condition.

Number of valid subarrays starting at `i`:
j - i


If no smaller element exists to the right:


n - i


We use a **monotonic increasing stack** to compute this efficiently.

---

## ⚙️ Algorithm Steps

- Traverse the array **from right to left**
- Maintain a **stack storing indices**
- Pop elements from the stack while:

arr[stack.peek()] >= arr[i]

- The next smaller element is either:
  - `stack.peek()` (if stack not empty)
  - `n` (if stack empty)
- Add `(nextSmallerIndex - i)` to the result
- Push the current index into the stack

---

## ✅ Java Solution

```java
import java.util.*;

class Solution {
    public int countSubarrays(int[] arr) {

        int count = 0;
        int n = arr.length;

        Stack<Integer> st = new Stack<>();

        for(int i = n - 1; i >= 0; i--) {

            while(!st.isEmpty() && arr[st.peek()] >= arr[i]) {
                st.pop();
            }

            if(st.isEmpty()) {
                count += n - i;
            } 
            else {
                count += st.peek() - i;
            }

            st.push(i);
        }

        return count;
    }
}
```
⏱️ Complexity Analysis

Complexity	Value

Time Complexity	O(n)

Space Complexity	O(n)

Each element is pushed and popped from the stack at most once.

Example

Input

arr = [1,3,5,2]

Output

8

Valid subarrays

[1]
[1,3]
[1,3,5]
[1,3,5,2]
[3]
[3,5]
[5]
[2]

 Key Concepts Used

Monotonic Stack

Next Smaller Element

Stack-based Array Processing

Subarray Counting

 Learning Outcome

This problem demonstrates how monotonic stacks help reduce brute-force complexity from O(n²) to O(n) by efficiently finding next smaller elements.

👨‍💻 Author

Shubham Patil

GitHub:
https://github.com/Shubham6502


---

If you want, I can also show you a **GitHub trick used by top DSA repositories** where every problem automatically shows:

⭐ Difficulty  
🏷️ Tags (Stack, Monotonic Stack, Array)  
🔗 GFG Link  

This makes your **DSA repo look very professional to recruiters.**
