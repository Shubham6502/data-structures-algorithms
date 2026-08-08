# Permutations Using Recursion

## Problem

Given an array of distinct integers, generate all possible permutations.

### Example

Input:

`[1, 2, 3]`

Output:

`[[1,2,3], [1,3,2], [2,1,3], [2,3,1], [3,1,2], [3,2,1]]`

## Approach

For every position, try every **unused element**.


Choose → Explore → Undo

We use a boolean[] used array to track which elements are already selected.

Java
```java
class Solution {

    public void find(
        int[] nums,
        List<Integer> curr,
        List<List<Integer>> ans,
        boolean[] used
    ) {
        if (curr.size() == nums.length) {
            ans.add(new ArrayList<>(curr));
            return;
        }

        for (int i = 0; i < nums.length; i++) {

            if (used[i]) continue;

            // Choose
            used[i] = true;
            curr.add(nums[i]);

            // Explore
            find(nums, curr, ans, used);

            // Undo / Backtrack
            curr.remove(curr.size() - 1);
            used[i] = false;
        }
    }

    public List<List<Integer>> permute(int[] nums) {
        List<List<Integer>> ans = new ArrayList<>();
        boolean[] used = new boolean[nums.length];

        find(nums, new ArrayList<>(), ans, used);

        return ans;
    }
}
``` 
Key Difference

Subset:

Take / Don't Take

Permutation:

Choose any unused element
Memory Trick
Permutation = Choose → Explore → Undo
Complexity
```
Time: O(n × n!)
Space: O(n) excluding the output
Number of permutations: n!
```
