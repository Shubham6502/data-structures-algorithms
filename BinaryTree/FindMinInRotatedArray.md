# Find Minimum in Rotated Sorted Array (LeetCode 153)

## Problem

Given a **sorted array** that has been **rotated** between `1` and `n` times, return the minimum element.

- All elements are **unique**.
- The solution must run in **O(log n)** time.

---

## Examples

### Example 1

```text
Input:
nums = [3,4,5,1,2]

Output:
1
```

---

### Example 2

```text
Input:
nums = [4,5,6,7,0,1,2]

Output:
0
```

---

### Example 3

```text
Input:
nums = [11,13,15,17]

Output:
11
```

---

# Intuition

A rotated sorted array consists of **two sorted halves**.

Example:

```text
Original

1 2 3 4 5 6 7
```

After rotation:

```text
4 5 6 7 1 2 3
```

Notice:

- Left half is sorted.
- Right half is sorted.
- The minimum element is where the rotation happened.

Instead of searching linearly, we can use **Binary Search**.

---

# Key Observation

If

```text
nums[left] <= nums[right]
```

then the current search space is already sorted.

Example

```text
2 3 5 8 10
```

Minimum is simply

```text
nums[left]
```

No further searching is required.

---

# Binary Search Logic

```java
while(left <= right){

    int mid = (left + right) / 2;

    if(nums[left] > nums[right]){

        if(nums[mid] > nums[right]){
            left = mid + 1;
        }else{
            right = mid;
        }

    }else{
        return nums[left];
    }
}
```

---

# Why Compare with `nums[right]`?

The last element belongs to the **right sorted portion**.

Example

```text
4 5 6 7 1 2 3
            ^
          right
```

If

```text
nums[mid] > nums[right]
```

then `mid` is in the **left sorted half**.

The minimum must be on the right.

Move

```text
left = mid + 1
```

---

Otherwise,

```text
nums[mid] <= nums[right]
```

means `mid` is in the right sorted half, and the minimum could be at `mid` itself.

Move

```text
right = mid
```

---
```java
class Solution {
    public int findMin(int[] nums) {
        int left=0;
        int right=nums.length-1;

        while(left<=right){
            int mid=(left+right)/2;
            if(nums[left]>nums[right]){
                if(nums[mid]>nums[right]){
                    left=mid+1;  
                }else{
                      right=mid;
                }
            }else{
                return nums[left];
            }
          

        }
        return nums[right];
    }
}
```
# Dry Run

## Example 1

```text
nums = [3,4,5,1,2]
```

Initial

```text
L              R
3 4 5 1 2
```

### Iteration 1

```text
mid = 2

L    M       R
3 4 5 1 2
```

```
nums[mid] = 5
nums[right] = 2
```

```
5 > 2
```

Minimum is on the right.

Move

```text
left = mid + 1
```

Now

```text
      L    R
3 4 5 1 2
```

---

### Iteration 2

```text
mid = 3

      L M  R
3 4 5 1 2
```

```
nums[mid] = 1
nums[right] = 2
```

```
1 <= 2
```

Minimum could be at `mid`.

Move

```text
right = mid
```

Now

```text
      LR
3 4 5 1 2
```

---

### Iteration 3

Now

```text
left == right
```

Current subarray

```text
1
```

Since

```text
nums[left] <= nums[right]
```

The search space is sorted.

Return

```text
1
```

---

# Dry Run

## Example 2

```text
nums = [4,5,6,7,0,1,2]
```

Initial

```text
L            R
4 5 6 7 0 1 2
```

---

### Iteration 1

```text
mid = 3

L      M      R
4 5 6 7 0 1 2
```

```
7 > 2
```

Move

```text
left = 4
```

---

### Iteration 2

```text
        L  M R
4 5 6 7 0 1 2
```

```
1 <= 2
```

Move

```text
right = mid
```

---

### Iteration 3

```text
        LM
4 5 6 7 0 1
```

Current range

```text
0 1
```

Already sorted.

Return

```text
0
```

---

# Already Sorted Array

```text
nums = [1,2,3,4,5]
```

Initially

```text
nums[left] <= nums[right]
```

```
1 <= 5
```

The array is already sorted.

Return

```text
1
```

No Binary Search is needed.

---

# Visualization

```text
Rotated Array

4 5 6 7 0 1 2
        ^
     Rotation Point

Minimum = 0
```

---

## Decision Tree

```text
                 left...right

          Is nums[left] <= nums[right] ?

                 /              \
              Yes                No
               |                  |
Return nums[left]      Compare nums[mid]
                              |
                nums[mid] > nums[right] ?
                        /             \
                     Yes              No
                      |                |
             left = mid+1      right = mid
```

---

# Why `right = mid` Instead of `mid - 1`?

When

```text
nums[mid] <= nums[right]
```

`mid` itself could be the minimum.

Example

```text
5 6 7 1 2 3
      ^
     mid
```

If we did

```text
right = mid - 1
```

we might skip the actual minimum.

So we keep

```text
right = mid
```

---

# Complexity Analysis

### Time Complexity

Binary Search halves the search space every iteration.

```text
O(log n)
```

---

### Space Complexity

Only a few variables are used.

```text
O(1)
```

---

# Key Takeaways

- A rotated sorted array consists of two sorted halves.
- If the current search space is already sorted (`nums[left] <= nums[right]`), the minimum is at `left`.
- Compare `nums[mid]` with `nums[right]` to determine which half contains the minimum.
- If `nums[mid] > nums[right]`, search the right half.
- Otherwise, search the left half while keeping `mid` because it may be the minimum.
- Binary Search achieves the required **O(log n)** time complexity.
