# Largest Rectangle in Histogram

**LeetCode:** 84  
**Difficulty:** Hard

---

# Problem Statement

Given an array `heights` representing the heights of histogram bars, where the width of each bar is `1`, return the area of the **largest rectangle** that can be formed in the histogram.

## Example 1

```text
Input:
heights = [2,1,5,6,2,3]

Output:
10
```

Explanation:

The largest rectangle is formed using bars of height `5` and `6`.

```text
        █
      █ █
      █ █
█     █ █     █
█     █ █ █   █
█ █ █ █ █ █   █
-----------------
2 1 5 6 2 3
```

Rectangle:

```text
      █ █
      █ █
Area = 5 × 2 = 10
```

---

## Example 2

```text
Input:
heights = [2,4]

Output:
4
```

---

# Intuition

For every bar, assume it is the **smallest bar** in the rectangle.

Then we need to know:

- How far can it extend to the **left**
- How far can it extend to the **right**

To find this efficiently:

- Find the **Previous Smaller Element (PSE)**
- Find the **Next Smaller Element (NSE)**

Using these two indices:

```text
Width = RightSmaller - LeftSmaller - 1
Area  = Height × Width
```

Take the maximum area among all bars.

---

# Visualization

Histogram

```text
Index    0 1 2 3 4 5
Height   2 1 5 6 2 3
```

For height = **5**

```text
Left Smaller  = index 1 (height = 1)
Right Smaller = index 4 (height = 2)

Rectangle

      █ █
      █ █
```

Width

```text
4 - 1 - 1 = 2
```

Area

```text
5 × 2 = 10
```

---

# Why do we use `-1` and `n`?

Suppose no smaller element exists on the left.

Instead of storing nothing, we store

```text
-1
```

Suppose no smaller element exists on the right.

We store

```text
n
```

Example

```text
heights = [2]
```

```text
leftMin  = -1
rightMin = 1 (n)
```

Width

```text
1 - (-1) - 1 = 1
```

Area

```text
1 × 2 = 2
```

If we stored

```text
rightMin = -1
```

Then

```text
Width = -1 - (-1) - 1 = -1
```

which is impossible.

So the virtual boundaries are

```text
Index

-1    0    1    2    n
 |    |    |    |    |

Height

 0    Histogram Bars    0
```

---

# Algorithm

### Step 1

Find the Previous Smaller Element (PSE) for every index using a monotonic increasing stack.

Example

```text
heights = [2,1,5,6,2,3]

leftMin

[-1,-1,1,2,1,4]
```

---

### Step 2

Find the Next Smaller Element (NSE).

```text
rightMin

[1,6,4,4,6,6]
```

---

### Step 3

For every index

```text
width = rightMin[i] - leftMin[i] - 1
area = width × heights[i]
```

Return the maximum area.

---

# Dry Run

Input

```text
heights = [2,1,5,6,2,3]
```

## Previous Smaller

| Index | Height | Previous Smaller |
|-------:|-------:|-----------------:|
|0|2|-1|
|1|1|-1|
|2|5|1|
|3|6|2|
|4|2|1|
|5|3|4|

---

## Next Smaller

| Index | Height | Next Smaller |
|-------:|-------:|-------------:|
|0|2|1|
|1|1|6|
|2|5|4|
|3|6|4|
|4|2|6|
|5|3|6|

---

## Area Calculation

| Height | Left | Right | Width | Area |
|---------|-----:|------:|------:|-----:|
|2|-1|1|1|2|
|1|-1|6|6|6|
|5|1|4|2|10|
|6|2|4|1|6|
|2|1|6|4|8|
|3|4|6|1|3|

Maximum Area

```text
10
```

---

# Java Solution

```java
class Solution {
    public int largestRectangleArea(int[] heights) {
        Stack<Integer> st = new Stack<>();
        int n = heights.length;

        int[] leftMin = new int[n];
        int[] rightMin = new int[n];

        // Previous Smaller Element
        for (int i = 0; i < n; i++) {
            while (!st.isEmpty() && heights[st.peek()] >= heights[i]) {
                st.pop();
            }

            leftMin[i] = st.isEmpty() ? -1 : st.peek();
            st.push(i);
        }

        st.clear();

        // Next Smaller Element
        for (int i = n - 1; i >= 0; i--) {
            while (!st.isEmpty() && heights[st.peek()] >= heights[i]) {
                st.pop();
            }

            rightMin[i] = st.isEmpty() ? n : st.peek();
            st.push(i);
        }

        int maxArea = 0;

        for (int i = 0; i < n; i++) {
            int width = rightMin[i] - leftMin[i] - 1;
            int area = width * heights[i];
            maxArea = Math.max(maxArea, area);
        }

        return maxArea;
    }
}
```

---

# Complexity Analysis

### Time Complexity

Finding Previous Smaller Elements:

```text
O(n)
```

Finding Next Smaller Elements:

```text
O(n)
```

Area calculation:

```text
O(n)
```

Overall

```text
O(n)
```

---

### Space Complexity

Two arrays:

```text
leftMin[]
rightMin[]
```

Stack

```text
Stack<Integer>
```

Overall

```text
O(n)
```

---

# Key Takeaways

- Use a **monotonic increasing stack**.
- Find the **Previous Smaller Element (PSE)** and **Next Smaller Element (NSE)**.
- Width is calculated as:

```text
rightMin - leftMin - 1
```

- Use virtual boundaries:
  - `-1` for the left boundary
  - `n` for the right boundary
- Every bar is treated as the **minimum height** of a possible rectangle.
- This approach solves the problem in **O(n)** time instead of the brute-force **O(n²)** approach.
