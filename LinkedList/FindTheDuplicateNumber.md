# Find the Duplicate Number

**Difficulty:** Medium  
**Topic:** Array, Two Pointers, Floyd's Cycle Detection (Tortoise and Hare)

---

## Problem Statement

Given an array `nums` containing `n + 1` integers where:

- Each integer is in the range `[1, n]`.
- There is **only one repeated number**, but it may appear multiple times.

Return the duplicate number **without modifying the array** and using only **constant extra space**.

---

## Intuition

At first, this problem looks like an array problem, but it can actually be viewed as a **Linked List**.

Each element represents the **index of the next node**.

For example:

```
nums = [1,3,4,2,2]
```

Index:

```
0   1   2   3   4
```

Values:

```
1   3   4   2   2
```

Connections:

```
0 → 1 → 3 → 2 → 4
          ↑     ↓
          └─────┘
```

Notice that a **cycle** is formed because the value `2` appears twice.

The duplicate number is exactly where the cycle begins.

---

# Floyd's Cycle Detection Algorithm

Floyd's Algorithm uses two pointers:

- **Slow Pointer** → Moves one step at a time.
- **Fast Pointer** → Moves two steps at a time.

It works in two phases.

---

## Phase 1: Detect the Cycle

Initialize:

```java
int slow = nums[0];
int fast = nums[0];
```

Move pointers:

```java
slow = nums[slow];
fast = nums[nums[fast]];
```

Eventually, both pointers meet inside the cycle.

---

## Phase 2: Find the Start of the Cycle

Reset the fast pointer:

```java
fast = nums[0];
```

Now move both pointers one step at a time.

```java
slow = nums[slow];
fast = nums[fast];
```

The point where they meet again is the **duplicate number**.

---

## Dry Run

### Input

```
nums = [1,3,4,2,2]
```

---

### Step 1: Build the Connections

```
0 → 1 → 3 → 2 → 4
          ↑     ↓
          └─────┘
```

Cycle starts at **2**.

---

### Phase 1

Initially

```
slow = 1
fast = 1
```

---

Move 1

```
slow = nums[1] = 3

fast = nums[nums[1]]
     = nums[3]
     = 2
```

```
slow = 3
fast = 2
```

---

Move 2

```
slow = nums[3] = 2

fast = nums[nums[2]]
     = nums[4]
     = 2
```

Now,

```
slow == fast
```

Meeting point:

```
2
```

---

### Phase 2

Reset

```
fast = nums[0] = 1
```

Current:

```
slow = 2
fast = 1
```

---

Move both one step

```
slow = nums[2] = 4

fast = nums[1] = 3
```

---

Move again

```
slow = nums[4] = 2

fast = nums[3] = 2
```

Both meet at

```
2
```

Answer:

```
Duplicate Number = 2
```

---

## Algorithm

1. Initialize both `slow` and `fast` with `nums[0]`.
2. Move:
   - `slow` by one step.
   - `fast` by two steps.
3. Continue until both pointers meet.
4. Reset `fast` to `nums[0]`.
5. Move both one step at a time.
6. The meeting point is the duplicate number.

---

## Code

```java
class Solution {
    public int findDuplicate(int[] nums) {

        int slow = nums[0];
        int fast = nums[0];

        // Phase 1: Detect the cycle
        do {
            slow = nums[slow];
            fast = nums[nums[fast]];
        } while (slow != fast);

        // Phase 2: Find the start of the cycle
        fast = nums[0];

        while (slow != fast) {
            slow = nums[slow];
            fast = nums[fast];
        }

        return slow;
    }
}
```

---

## Complexity Analysis

### Time Complexity

- Phase 1: **O(n)**
- Phase 2: **O(n)**

Overall:

```
O(n)
```

---

### Space Complexity

Only two pointers are used.

```
O(1)
```

---

## Why Does This Work?

Since the array has:

- `n + 1` elements
- Values only from `1` to `n`

At least one value must repeat (Pigeonhole Principle).

That repeated value causes two indices to point to the same next position, creating a cycle.

Floyd's Cycle Detection finds:

1. A meeting point inside the cycle.
2. The starting point of the cycle.

The start of the cycle is exactly the duplicate number.

---

## Key Insight

Think of the array as a linked list:

```
Index  ---> Next Index
```

Example:

```
nums = [1,3,4,2,2]

0 → 1 → 3 → 2 → 4
          ↑     ↓
          └─────┘
```

The duplicate number creates a cycle, and Floyd's Algorithm finds the beginning of that cycle.

---

## Interview Tips

- This is **not** a traditional linked list problem.
- The array is treated as a **graph of pointers**.
- Floyd's Algorithm is commonly known as the **Tortoise and Hare Algorithm**.
- No sorting, hashing, or extra array is needed.
- This satisfies the constraints:
  - Array remains unchanged.
  - Constant extra space.
  - Linear time.

---

## Similar Problems

- Linked List Cycle
- Linked List Cycle II
- Happy Number
- Detect Cycle in Directed Graph (Concept)
