# Palindrome Partitioning

## Problem

Given a string `s`, partition it so that every substring in the partition is a palindrome.

### Example

**Input:**

```text
s = "aab"
```

**Output:**

```text
[["a","a","b"],["aa","b"]]
```

## Approach

Use **Backtracking**.

* `idx` represents the starting position of the current substring.
* Try every possible ending position `i`.
* Check whether `s[idx...i]` is a palindrome.
* If it is, add it to `curr`.
* Recursively process the remaining string.
* Remove the substring after recursion to backtrack.

### Important Point

After choosing `s[idx...i]`, the next recursion starts from:

```java
find(s, i + 1, curr, ans);
```

Because `i` is the last index already used.

## Code

```java
class Solution {

    public boolean isPallindrome(String str, int i, int j) {

        while(i < j) {
            if(str.charAt(i) != str.charAt(j)) {
                return false;
            }
            i++;
            j--;
        }

        return true;
    }

    public void find(String s, int idx,
                     List<String> curr,
                     List<List<String>> ans) {

        if(idx == s.length()) {
            ans.add(new ArrayList<>(curr));
            return;
        }

        for(int i = idx; i < s.length(); i++) {

            if(isPallindrome(s, idx, i)) {

                curr.add(s.substring(idx, i + 1));

                find(s, i + 1, curr, ans);

                curr.remove(curr.size() - 1);
            }
        }
    }

    public List<List<String>> partition(String s) {

        List<List<String>> ans = new ArrayList<>();

        find(s, 0, new ArrayList<String>(), ans);

        return ans;
    }
}
```

## Backtracking

```text
Choose → Explore → Undo
```

For `aab`:

```text
aab
├── a
│   ├── a
│   │   └── b
│   └── ab ❌
└── aa
    └── b
```

Valid partitions:

```text
["a", "a", "b"]
["aa", "b"]
```

## Complexity

* **Time:** `O(n × 2^n)` approximately
* **Space:** `O(n)` recursion depth, excluding the output
