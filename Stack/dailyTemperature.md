# 739. Daily Temperatures

## Problem Statement

Given an array `temperatures` where `temperatures[i]` represents the temperature on the `i-th` day, return an array `answer` such that:

- `answer[i]` is the number of days you have to wait after the `i-th` day to get a warmer temperature.
- If there is no future day with a warmer temperature, return `0` for that day.

### Example

**Input**

```text
temperatures = [73,74,75,71,69,72,76,73]
```

**Output**

```text
[1,1,4,2,1,1,0,0]
```

---

## Approach

This solution uses a **Monotonic Decreasing Stack**.

Instead of checking every future day (which would take `O(n²)`), we process the array **from right to left**.

### Key Idea

- The stack stores **indices** of temperatures.
- Maintain the stack such that temperatures are in **strictly decreasing order**.
- While the current temperature is greater than or equal to the temperature at the top index of the stack, remove that index because it can never be the next warmer day.
- After removing smaller temperatures:
  - If the stack is empty, no warmer day exists.
  - Otherwise, the top index is the next warmer day.
- Push the current index onto the stack.

Finally, convert the stored index into the number of waiting days by subtracting the current index.

---

## Algorithm

1. Create a result array.
2. Create an empty stack to store indices.
3. Traverse the array from right to left.
4. Remove all indices whose temperatures are less than or equal to the current temperature.
5. If the stack is not empty, store the top index.
6. Push the current index onto the stack.
7. Convert stored indices into waiting days.
8. Return the result.

---

## Dry Run

### Input

```text
temperatures = [73,74,75,71,69,72,76,73]
```

| Index | Temp | Stack (Indices) | Result |
|------:|-----:|----------------:|-------:|
| 7 | 73 | [7] | 0 |
| 6 | 76 | [6] | 0 |
| 5 | 72 | [6,5] | 6 |
| 4 | 69 | [6,5,4] | 5 |
| 3 | 71 | [6,5,3] | 5 |
| 2 | 75 | [6,2] | 6 |
| 1 | 74 | [6,2,1] | 2 |
| 0 | 73 | [6,2,1,0] | 1 |

Convert indices into day differences:

```text
[1,1,4,2,1,1,0,0]
```

---

## Java Solution

```java
class Solution {
    public int[] dailyTemperatures(int[] temperatures) {
        int n = temperatures.length;
        int[] res = new int[n];
        Stack<Integer> st = new Stack<>();

        for (int i = n - 1; i >= 0; i--) {

            while (!st.isEmpty() &&
                   temperatures[st.peek()] <= temperatures[i]) {
                st.pop();
            }

            res[i] = st.isEmpty() ? 0 : st.peek();

            st.push(i);
        }

        for (int i = 0; i < n; i++) {
            if (res[i] != 0)
                res[i] = res[i] - i;
        }

        return res;
    }
}
```

---

## Complexity Analysis

- **Time Complexity:** `O(n)`
  - Each index is pushed and popped from the stack at most once.

- **Space Complexity:** `O(n)`
  - Stack may store all indices in the worst case.

---

## Key Concepts

- Monotonic Decreasing Stack
- Stack of Indices
- Reverse Traversal
- Next Greater Element Pattern
- Efficient `O(n)` Solution
