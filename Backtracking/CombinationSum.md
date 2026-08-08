# Combination Sum — Recursion

## Problem

Given an array of distinct integers `candidates` and a target integer `target`, return all unique combinations where the chosen numbers sum to `target`.

The same number can be chosen unlimited times.

### Example

Input:

`candidates = [2, 3, 6, 7]`  
`target = 7`

Output:

`[[2, 2, 3], [7]]`

---

## Key Idea

For every element, we have two choices:

1. Take the current element.
2. Don't take the current element.

The important difference from normal subset recursion is:

**Take → stay at the same index**

**Don't Take → move to the next index**

Because the same element can be used multiple times.

---

## Recursion Pattern

```text
                 Current Element
                 /             \
              TAKE           DON'T TAKE
                |                 |
             same idx          idx + 1
                |                 |
             recursion         recursion
Memory Trick
TAKE
 ↓
Same index

DON'T TAKE
 ↓
Next index
```
Java Solution
```java
class Solution {

    public void findSum(
        int[] arr,
        int target,
        int sum,
        int idx,
        List<Integer> curr,
        List<List<Integer>> ans
    ) {

        // Base case
        if (sum == target) {
            ans.add(new ArrayList<>(curr));
            return;
        }

        // Invalid case
        if (idx >= arr.length || sum > target) {
            return;
        }

        // Take current element
        curr.add(arr[idx]);

        findSum(
            arr,
            target,
            sum + arr[idx],
            idx,
            curr,
            ans
        );

        // Backtrack
        curr.remove(curr.size() - 1);

        // Don't take current element
        findSum(
            arr,
            target,
            sum,
            idx + 1,
            curr,
            ans
        );
    }

    public List<List<Integer>> combinationSum(int[] candidates, int target) {

        List<List<Integer>> ans = new ArrayList<>();

        findSum(
            candidates,
            target,
            0,
            0,
            new ArrayList<>(),
            ans
        );

        return ans;
    }
}
```
How It Works

Suppose:

candidates = [2, 3, 6, 7]

target = 7

Initially:

sum = 0
idx = 0
curr = []

The current element is 2.

Take 2

We add 2:

curr = [2]
sum = 2
idx = 0

The index remains 0 because we can use 2 again.

Take 2 again:

curr = [2, 2]
sum = 4
idx = 0

Take 2 again:

curr = [2, 2, 2]
sum = 6
idx = 0

Take 2 again:

sum = 8

Since:

8 > 7

we return.

Backtracking

After exploring a choice, we undo it:

curr.remove(curr.size() - 1);

For example:

[2, 2, 2]

becomes:

[2, 2]

Then we continue exploring other possibilities.

This is called backtracking.

Remember
Choose
   ↓
Explore
   ↓
Undo
Why We Use sum + arr[idx]

We should avoid changing sum directly:

sum += arr[idx];

findSum(...);

findSum(..., sum, ...);

Because now the original sum has been changed.

Instead, pass the new value directly:

findSum(
    arr,
    target,
    sum + arr[idx],
    idx,
    curr,
    ans
);

This means:

Take branch gets sum + arr[idx]
Don't-take branch gets the original sum

Example:

Current sum = 4
Current element = 3

TAKE:
sum = 4 + 3 = 7

DON'T TAKE:
sum = 4
Why Does TAKE Use the Same Index?

Because the same number can be used unlimited times.

Example:

candidates = [2, 3]
target = 7

A valid combination is:

2 + 2 + 3 = 7

So after taking 2, we stay at the same index:

findSum(
    arr,
    target,
    sum + arr[idx],
    idx,
    curr,
    ans
);

Therefore:

TAKE → idx
Why Does DON'T TAKE Use idx + 1?

If we don't want to use the current element, move to the next candidate:

findSum(
    arr,
    target,
    sum,
    idx + 1,
    curr,
    ans
);

Therefore:

DON'T TAKE → idx + 1
Base Cases
1. Target Reached
if (sum == target) {
    ans.add(new ArrayList<>(curr));
    return;
}

If the current sum equals the target, we found a valid combination.

Example:

curr = [2, 2, 3]
sum = 7

Store the combination and return.

2. Sum Exceeds Target
if (sum > target) {
    return;
}

Since all candidates are positive, there is no reason to continue.

3. Array Ends
if (idx >= arr.length) {
    return;
}

There are no more elements to process.

Recursion Tree Idea

For each element, there are two choices:

                    []
                  /    \
              TAKE    DON'T TAKE
                2          2
                |
              [2]
             /   \
         TAKE   DON'T TAKE
           2         3
           |
         [2,2]

The recursion keeps exploring until:

sum == target

or:

sum > target

or:
```
idx >= arr.length
```
Difference Between Subsets and Combination Sum
Subsets

Every element is used at most once.
```
TAKE        → idx + 1
DON'T TAKE  → idx + 1
```
Combination Sum

The same element can be used multiple times.
```
TAKE        → idx
DON'T TAKE  → idx + 1
Most Important Difference
```
SUBSET:

TAKE → next index


COMBINATION SUM:

TAKE → same index
