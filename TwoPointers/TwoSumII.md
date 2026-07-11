# Two Sum II - Input Array Is Sorted

## Problem Statement

Given a **1-indexed** array of integers `numbers` that is already sorted in non-decreasing order, find two numbers such that their sum is equal to `target`.

Return the indices of the two numbers as:

```text
[index1, index2]
```

where:

```text
1 <= index1 < index2 <= numbers.length
```

---

## Example

### Input

```text
numbers = [2, 7, 11, 15]
target = 9
```

### Output

```text
[1, 2]
```

### Explanation

```text
numbers[0] + numbers[1]
= 2 + 7
= 9
```

The problem requires **1-based indexing**, so we return:

```text
[1, 2]
```

---

## Approach: Two Pointers

Since the array is already sorted, we can use the **Two Pointer** technique.

We initialize:

```text
i = 0                  → Left pointer
j = numbers.length - 1 → Right pointer
```

Then calculate:

```text
sum = numbers[i] + numbers[j]
```

There are three possible cases:

### Case 1: `sum == target`

We found the answer.

```java
return new int[]{i + 1, j + 1};
```

We add `1` because the answer requires **1-based indices**.

### Case 2: `sum > target`

The sum is too large.

Since the array is sorted, we move the right pointer to a smaller number:

```java
j--;
```

### Case 3: `sum < target`

The sum is too small.

We move the left pointer to a larger number:

```java
i++;
```

---

## Dry Run

### Input

```text
numbers = [2, 7, 11, 15]
target = 9
```

### Initial State

```text
[2, 7, 11, 15]
 ↑          ↑
 i          j
```

```text
i = 0
j = 3

sum = 2 + 15 = 17
```

Since:

```text
17 > 9
```

Move `j` to the left:

```text
[2, 7, 11, 15]
 ↑      ↑
 i      j
```

Now:

```text
i = 0
j = 2

sum = 2 + 11 = 13
```

Since:

```text
13 > 9
```

Move `j` to the left again:

```text
[2, 7, 11, 15]
 ↑  ↑
 i  j
```

Now:

```text
i = 0
j = 1

sum = 2 + 7 = 9
```

Since:

```text
sum == target
```

Return:

```text
[i + 1, j + 1]
= [1, 2]
```

---

## Java Solution

```java
class Solution {
    public int[] twoSum(int[] numbers, int target) {
        int i = 0;
        int j = numbers.length - 1;

        while (i < j) {
            int sum = numbers[i] + numbers[j];

            if (sum == target) {
                return new int[]{i + 1, j + 1};
            } else if (sum > target) {
                j--;
            } else {
                i++;
            }
        }

        return new int[]{-1, -1};
    }
}
```

---

## Why Does the Two-Pointer Approach Work?

The array is sorted:

```text
Smallest ------------------------> Largest
```

Therefore:

* If the current sum is **too large**, moving `j` left decreases the sum.
* If the current sum is **too small**, moving `i` right increases the sum.

This allows us to find the answer without checking every possible pair.

---

## Complexity Analysis

### Time Complexity

```text
O(n)
```

Each pointer moves at most `n` times.

### Space Complexity

```text
O(1)
```

Only two pointer variables are used, so no extra data structure is required.

---

## Key Concepts

* Two Pointers
* Sorted Array
* Array Traversal
* 1-Based Indexing
* Space Optimization

---

## Test Cases

### Test Case 1

```text
Input:
numbers = [2, 7, 11, 15]
target = 9

Output:
[1, 2]
```

### Test Case 2

```text
Input:
numbers = [2, 3, 4]
target = 6

Output:
[1, 3]
```

### Test Case 3

```text
Input:
numbers = [-1, 0]
target = -1

Output:
[1, 2]
```
