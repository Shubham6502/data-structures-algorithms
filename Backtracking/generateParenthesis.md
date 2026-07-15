# 22. Generate Parentheses

## Problem

Given `n` pairs of parentheses, generate all combinations of **well-formed parentheses**.

### Example

**Input:**

```text
n = 3
```

**Output:**

```text
["((()))", "(()())", "(())()", "()(())", "()()()"]
```

---

## Approach: Backtracking

We build the parentheses string one character at a time.

At every step, we have two possible choices:

1. Add an opening parenthesis `(`
2. Add a closing parenthesis `)`

However, we must follow two rules to ensure that only valid combinations are generated.

### Rule 1: Add `(` only when `a < n`

`a` represents the number of opening parentheses used.

```java
if (a < n)
```

We can use at most `n` opening parentheses.

### Rule 2: Add `)` only when `b < a`

`b` represents the number of closing parentheses used.

```java
if (b < a)
```

A closing parenthesis can only be added when there is an unmatched opening parenthesis available.

For example:

```text
Current string: "("

a = 1
b = 0

b < a
0 < 1 → true

So we can add ')'
```

But for:

```text
Current string: "()"

a = 1
b = 1

b < a
1 < 1 → false

So we cannot add another ')'
```

This prevents invalid combinations such as:

```text
())
)(
))((
```

---

## Backtracking Process

For every valid choice, we follow three steps:

```text
1. Choose
2. Explore
3. Undo
```

### Adding `(`

```java
sb.append('(');                         // Choose
parenthesis(n, a + 1, b, list, sb);    // Explore
sb.deleteCharAt(sb.length() - 1);       // Undo
```

### Adding `)`

```java
sb.append(')');                         // Choose
parenthesis(n, a, b + 1, list, sb);    // Explore
sb.deleteCharAt(sb.length() - 1);       // Undo
```

The `deleteCharAt()` operation is the **backtracking step**. It restores the previous state so that another possible path can be explored.

---

## Recursion Tree for `n = 2`

```text
                         ""
                         |
                        "("
                       /   \
                     "(("   "()"
                      |       |
                    "(()"   "()("
                      |       |
                   "(())"   "()()"
                     ✅       ✅
```

The recursive traversal happens depth-first:

```text
""
 ↓
"("
 ↓
"(("
 ↓
"(()"
 ↓
"(())" ✅
 ↑
"(()"
 ↑
"(("
 ↑
"("
 ↓
"()"
 ↓
"()("
 ↓
"()()" ✅
```

Here:

```text
↓ = Recursive call / go deeper
↑ = Return / backtrack
```

---

## Code

```java
import java.util.*;

class Solution {

    public void parenthesis(
        int n,
        int a,
        int b,
        List<String> list,
        StringBuilder sb
    ) {

        // Base case:
        // A complete combination contains n opening
        // and n closing parentheses.
        if (sb.length() == n * 2) {
            list.add(sb.toString());
            return;
        }

        // Add '(' if fewer than n opening parentheses are used.
        if (a < n) {
            sb.append('(');

            parenthesis(n, a + 1, b, list, sb);

            // Backtrack
            sb.deleteCharAt(sb.length() - 1);
        }

        // Add ')' only if there is an unmatched '('.
        if (b < a) {
            sb.append(')');

            parenthesis(n, a, b + 1, list, sb);

            // Backtrack
            sb.deleteCharAt(sb.length() - 1);
        }
    }

    public List<String> generateParenthesis(int n) {

        List<String> list = new ArrayList<>();

        parenthesis(
            n,
            0,
            0,
            list,
            new StringBuilder()
        );

        return list;
    }
}
```

---

## Variable Meaning

| Variable | Meaning                                |
| -------- | -------------------------------------- |
| `n`      | Number of pairs of parentheses         |
| `a`      | Number of opening parentheses `(` used |
| `b`      | Number of closing parentheses `)` used |
| `list`   | Stores all valid combinations          |
| `sb`     | Builds the current combination         |

---

## Dry Run for `n = 2`

### Initial Call

```text
parenthesis(2, 0, 0, "")
```

### First Path

```text
""        a=0, b=0
 ↓ add '('
"("       a=1, b=0
 ↓ add '('
"(("      a=2, b=0
 ↓ add ')'
"(()"     a=2, b=1
 ↓ add ')'
"(())"    a=2, b=2 ✅
```

Add `(())` to the result.

Then backtrack:

```text
"(())"
  ↓ undo
"(()"
  ↓ return
"(("
  ↓ return
"("
```

Now try the other possible choice from `"("`.

### Second Path

```text
"("       a=1, b=0
 ↓ add ')'
"()"      a=1, b=1
 ↓ add '('
"()("     a=2, b=1
 ↓ add ')'
"()()"    a=2, b=2 ✅
```

Final result:

```text
["(())", "()()"]
```

---

## Why `b < a`?

We want the final result to have:

```text
a == b == n
```

But while building the string, before adding a closing parenthesis `)`, there must already be an unmatched opening parenthesis `(`.

Therefore:

```text
Before adding ')':

b < a
```

After adding `)`:

```text
b <= a
```

At the end:

```text
a == b == n
```

---

## Time Complexity

The number of valid parentheses combinations is given by the **Catalan number**.

The time complexity is approximately:

```text
O(4^n / √n)
```

Since each valid result has a length of `2n`, the total work including building the output can be written as:

```text
O(n × Catalan(n))
```

---

## Space Complexity

```text
O(n)
```

The maximum recursion depth is `2n`, and the `StringBuilder` also stores at most `2n` characters.

The output list is not included in the auxiliary space complexity.

---

## Key Takeaway

The backtracking pattern used in this problem is:

```text
MAKE A CHOICE
      ↓
EXPLORE WITH RECURSION
      ↓
UNDO THE CHOICE
      ↓
TRY THE NEXT CHOICE
```

In code:

```java
sb.append(choice);                    // Choose

parenthesis(...);                     // Explore

sb.deleteCharAt(sb.length() - 1);     // Undo / Backtrack
```

This allows the same `StringBuilder` to be reused while exploring every valid path.
