# Letter Combinations of a Phone Number

## Problem

Given a string containing digits from `2-9`, return all possible letter combinations that the number could represent.

The mapping is the same as a telephone keypad:

```text
2 → abc
3 → def
4 → ghi
5 → jkl
6 → mno
7 → pqrs
8 → tuv
9 → wxyz
```

## Approach

Use **backtracking** to generate every possible combination.

1. Store the digit-to-letter mapping in a string array.
2. Start from index `0` of the input digits.
3. Get the letters corresponding to the current digit.
4. Add each letter to `curr`.
5. Recursively process the next digit.
6. After recursion, remove the last character using backtracking.
7. When `idx == digit.length()`, add the completed string to `ans`.

## Java Solution

```java
class Solution {

    String str[] = {"", "", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"};

    public void combination(String digit, int idx, List<String> ans, StringBuilder curr) {

        // Base case
        if (idx == digit.length()) {
            ans.add(curr.toString());
            return;
        }

        // Get letters for current digit
        String charStr = str[digit.charAt(idx) - '0'];

        // Try every possible character
        for (char ch : charStr.toCharArray()) {

            // Choose
            curr.append(ch);

            // Explore
            combination(digit, idx + 1, ans, curr);

            // Backtrack
            curr.deleteCharAt(curr.length() - 1);
        }
    }

    public List<String> letterCombinations(String digits) {

        List<String> ans = new ArrayList<>();

        combination(digits, 0, ans, new StringBuilder());

        return ans;
    }
}
```

## Dry Run

For:

```text
digits = "23"
```

Mapping:

```text
2 → abc
3 → def
```

The recursion generates:

```text
ad
ae
af
bd
be
bf
cd
ce
cf
```

So the result is:

```text
["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"]
```

## Backtracking Concept

The important part is:

```java
curr.append(ch);

combination(digit, idx + 1, ans, curr);

curr.deleteCharAt(curr.length() - 1);
```

Think of it as:

```text
Choose
  ↓
Explore
  ↓
Undo
```

For example:

```text
a
├── ad
├── ae
└── af

b
├── bd
├── be
└── bf

c
├── cd
├── ce
└── cf
```

After completing `ad`, we remove `d` and try `e`.

```text
"ad"
 ↓ remove d
"a"
 ↓ add e
"ae"
```

This is the core idea of **backtracking**.

## Complexity

Let `n` be the number of digits.

Each digit can have up to `4` letters.

### Time Complexity

```text
O(4^n × n)
```

There are at most `4^n` combinations, and creating/storing each combination takes `O(n)` time.

### Space Complexity

```text
O(n)
```

for the recursion stack and current `StringBuilder`.

Additionally, the output itself requires:

```text
O(4^n × n)
```

space.

## Important Edge Case

If the input is empty:

```text
digits = ""
```

the current implementation returns:

```text
[""]
```

If the expected LeetCode behavior is an empty list, add:

```java
if (digits.length() == 0) {
    return ans;
}
```

So the final method can be:

```java
public List<String> letterCombinations(String digits) {

    List<String> ans = new ArrayList<>();

    if (digits.length() == 0) {
        return ans;
    }

    combination(digits, 0, ans, new StringBuilder());

    return ans;
}
```

## Key Interview Point

The main concept to explain in an interview:

> "I use backtracking to build the combination character by character. For every digit, I try all its possible letters, recursively process the next digit, and then remove the chosen character so I can try the next possibility."
