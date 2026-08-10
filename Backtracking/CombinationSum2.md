# Combination Sum II

## Problem

Given a collection of candidate numbers and a target integer, find all unique combinations where the chosen numbers sum to the target.

Each number in `candidates` may be used **at most once**.

The solution must not contain duplicate combinations.

### Example

**Input:**
```text
candidates = [10,1,2,7,6,1,5]
target = 8

Output:

[[1,1,6],[1,2,5],[1,7],[2,6]]
```
Approach

This problem can be solved using Backtracking.

1. Sort the array

First, sort the candidates:
```
Arrays.sort(candidates);
```

Sorting helps us:

Detect duplicate values easily.
Skip duplicate choices.
Stop recursion when the current sum becomes greater than the target.

For example:

[1,2,2,5,6,7,10]


2. Include or Exclude

For every element, we have two choices:

Include the current element
        OR
Exclude the current element
Include

Add the current candidate to curr:

curr.add(candidates[idx]);

Then recursively move to the next index:
```
find(candidates, idx + 1, target,
     total + candidates[idx], curr, res);
```
We use idx + 1 because every element can be used only once.

After recursion, backtrack:
```

curr.remove(curr.size() - 1);
```
3. Exclude Duplicate Values

After exploring the include branch, we skip consecutive duplicate values:
```
while(idx < candidates.length - 1 &&
      candidates[idx] == candidates[idx + 1]) {
    idx++;
}
```
Then explore the exclude branch:
```
find(candidates, idx + 1, target, total, curr, res);
```
This prevents duplicate combinations.

Important

We skip duplicates only when moving to the exclude branch.

Why?

Suppose:

[1,2,2]

The two 2s can both be used in the same combination:

[2,2]

So we cannot simply skip every duplicate.

We skip the duplicate only when deciding:

"I don't want to take this value at this recursion level."

Base Cases
Target reached

If:
```java
total == target
```
we found a valid combination:
```
res.add(new ArrayList<>(curr));
return;
```
We create a new ArrayList because curr will later be modified during backtracking.

Invalid state

Stop recursion when:
```
if(idx >= candidates.length || total > target) return;
```
This means:

We have reached the end of the array.
The current sum is already greater than the target.
Code
```java
class Solution {

    public void find(
        int[] candidates,
        int idx,
        int target,
        int total,
        List<Integer> curr,
        List<List<Integer>> res
    ) {

        if (total == target) {
            res.add(new ArrayList<>(curr));
            return;
        }

        if (idx >= candidates.length || total > target) {
            return;
        }

        // Include current element
        curr.add(candidates[idx]);

        find(
            candidates,
            idx + 1,
            target,
            total + candidates[idx],
            curr,
            res
        );

        // Backtrack
        curr.remove(curr.size() - 1);

        // Skip duplicate values for exclude branch
        while (
            idx < candidates.length - 1 &&
            candidates[idx] == candidates[idx + 1]
        ) {
            idx++;
        }

        // Exclude current element
        find(
            candidates,
            idx + 1,
            target,
            total,
            curr,
            res
        );
    }

    public List<List<Integer>> combinationSum2(
        int[] candidates,
        int target
    ) {

        List<List<Integer>> res = new ArrayList<>();

        Arrays.sort(candidates);

        find(
            candidates,
            0,
            target,
            0,
            new ArrayList<>(),
            res
        );

        return res;
    }

```
Recursion Tree Example

For:

candidates = [1,2,2]
target = 3

The important idea is:

                     []
                  /      \
               take 1    skip 1
                 /          \
               [1]          ...
              /   \
         take 2   skip 2
          /          \
       [1,2]         [1]
         |             |
      take 2        skip duplicate 2
         |
      [1,2,2]

When the first 2 is skipped, the second 2 is also skipped for the exclude decision at that level, preventing duplicate combinations.

However, when we choose the first 2, we can still choose the second 2:

[2,2]

That's why the duplicate-skipping logic is placed after backtracking, before the exclude recursive call.

Complexity

Let N be the number of candidates.

Time Complexity

Worst case:
```
O(2^N)
```
because each element can potentially be included or excluded.

Sorting takes:
```
O(N log N)
```
So overall:
```
O(N log N + 2^N)
```
Space Complexity

The recursion depth can be at most N:
```
O(N)
```
excluding the space required to store the result.
