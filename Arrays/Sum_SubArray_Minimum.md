# Sum of Subarray Minimums (GFG)

## Problem

Given an array `arr[]` of positive integers, find the **total sum of the minimum elements of every possible subarray**.

### Example

Input:
```
arr = [3,1,2,4]
```

Subarrays and their minimum values:

| Subarray | Minimum |
|--------|--------|
| [3] | 3 |
| [3,1] | 1 |
| [3,1,2] | 1 |
| [3,1,2,4] | 1 |
| [1] | 1 |
| [1,2] | 1 |
| [1,2,4] | 1 |
| [2] | 2 |
| [2,4] | 2 |
| [4] | 4 |

Total Sum:

```
3 + 1 + 1 + 1 + 1 + 1 + 1 + 2 + 2 + 4 = 17
```

Output:
```
17
```

---

# Approach (Monotonic Stack)

Instead of generating all subarrays (which takes **O(n²)**), we calculate **how many subarrays use `arr[i]` as the minimum**.

For every element:

```
Contribution = arr[i] × left × right
```

Where:

- `left` = number of elements to the left where `arr[i]` is the minimum
- `right` = number of elements to the right where `arr[i]` remains the minimum

Then add all contributions to get the final answer.

---

# Algorithm

1. Find **Previous Smaller Element** for each index → determines `left`.
2. Find **Next Smaller Element** for each index → determines `right`.
3. Use **Monotonic Increasing Stack**.
4. Calculate contribution of every element.

---

# Java Implementation

```java
import java.util.*;

class Solution {
    public int sumSubMins(int[] arr) {
        int sum = 0;
        int n = arr.length;

        int left[] = new int[n];
        int right[] = new int[n];

        Stack<Integer> st = new Stack<>();

        // Previous Smaller Element
        for(int i = 0; i < n; i++){
            while(!st.isEmpty() && arr[st.peek()] > arr[i]){
                st.pop();
            }

            if(st.isEmpty()){
                left[i] = i + 1;
            }else{
                left[i] = i - st.peek();
            }

            st.push(i);
        }

        st.clear();

        // Next Smaller Element
        for(int i = n - 1; i >= 0; i--){
            while(!st.isEmpty() && arr[st.peek()] >= arr[i]){
                st.pop();
            }

            if(st.isEmpty()){
                right[i] = n - i;
            }else{
                right[i] = st.peek() - i;
            }

            st.push(i);
        }

        // Calculate contribution
        for(int i = 0; i < n; i++){
            sum += arr[i] * left[i] * right[i];
        }

        return sum;
    }
}
```

---

# Complexity Analysis

| Type | Complexity |
|------|------------|
| Time Complexity | **O(n)** |
| Space Complexity | **O(n)** |

---

# Key Concept

This problem uses **Monotonic Stack**, which is useful for problems like:

- Next Smaller Element
- Previous Smaller Element
- Largest Rectangle in Histogram
- Sum of Subarray Minimums
- Stock Span Problem

---

# Interview Tip

Whenever a problem asks about:

- **Nearest greater/smaller element**
- **Range contribution of elements**

Think about:

```
Monotonic Stack
```
