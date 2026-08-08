# Subsets Using Recursion

## Problem

Given an array of integers, generate all possible subsets of the array.

### Example

**Input:**

```text
[1, 2, 3]

Output:

[]
[1]
[2]
[3]
[1, 2]
[1, 3]
[2, 3]
[1, 2, 3]
```
Key Idea

For every element, we have exactly two choices:

Take the current element.
Don't take the current element.

This creates a recursion tree.

                 []
              /      \
            [1]       []
           /   \      /  \
        [1,2] [1]   [2]  []

Every path from the root to the end represents one subset.

Recursive Approach

We maintain:

index → current element
current → subset currently being built
ans → stores all subsets
Base Case

When we have processed all elements
```java

if (index == nums.length) {
    ans.add(new ArrayList<>(current));
    return;
}
```

We copy current because the same list is modified during backtracking.
```java
Java Solution
class Solution {

    public List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> ans = new ArrayList<>();

        generate(0, nums, new ArrayList<>(), ans);

        return ans;
    }

    void generate(int index, int[] nums,
                  List<Integer> current,
                  List<List<Integer>> ans) {

        // Base case
        if (index == nums.length) {
            ans.add(new ArrayList<>(current));
            return;
        }

        // 1. Take the current element
        current.add(nums[index]);

        generate(index + 1, nums, current, ans);

        // Backtrack
        current.remove(current.size() - 1);

        // 2. Don't take the current element
        generate(index + 1, nums, current, ans);
    }
}
```
How Recursion Works

For every element:

                 TAKE
                  |
             index + 1
```
current element
             |
             |
                 DON'T TAKE
                  |
             index + 1

So the general pattern is:

take();
recursion();

backtrack();

don'tTake();
recursion();
Example: [1, 2]

Start:

index = 0
current = []
Take 1
current = [1]

Then process 2.

Take 2:

[1, 2]

Don't take 2:

[1]
Backtrack

Remove 1:

[]

Don't take 1.

Then process 2.

Take 2:

[2]

Don't take 2:

[]

Final answer:

[[], [1], [2], [1,2]]
Why Do We Backtrack?

After exploring:

[1, 2]

we need to return to:

[1]

So we remove 2.

current.remove(current.size() - 1);

Similarly, after finishing all possibilities with 1, we remove 1.

This is called backtracking.

Recursion Pattern to Remember

Whenever you see a problem involving:

Subsets
Subsequences
Combinations
Take / Don't Take
Include / Exclude
```

Think:

             Current Element
              /          \
           TAKE        DON'T TAKE
            |              |
       index + 1       index + 1
Memory Trick

Remember:

Every element → 2 choices → Take or Don't Take

And for backtracking:

Choose → Explore → Undo

Choose
  ↓
Recursion
  ↓
Undo
Complexity

For n elements, every element has 2 choices:

2 × 2 × 2 × ... × 2 = 2^n

There are 2^n possible subsets.

Time Complexity
O(n × 2^n)

We have 2^n subsets, and copying each subset can take up to O(n).

Space Complexity
O(n)

for the recursion stack and current subset, excluding the output.

If we include the output:

O(n × 2^n)
Important Recursion Template
```java
void solve(int index, int[] nums,
           List<Integer> current,
           List<List<Integer>> ans) {

    if (index == nums.length) {
        ans.add(new ArrayList<>(current));
        return;
    }

    // Take
    current.add(nums[index]);
    solve(index + 1, nums, current, ans);

    // Undo
    current.remove(current.size() - 1);

    // Don't take
    solve(index + 1, nums, current, ans);
}
```
One-Line Concept

Subset recursion = Take + Don't Take + Backtrack
