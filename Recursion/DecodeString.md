# 🔓 Decode String

## 📌 Problem Statement

Given an encoded string, return its decoded string.

The encoding rule is:

```text
k[encoded_string]
```

where `encoded_string` inside the square brackets is repeated exactly `k` times.

### Example

```text
Input:  s = "3[a2[c]]"
Output: "accaccacc"
```

### Explanation

```text
2[c]      → "cc"
a2[c]     → "acc"
3[a2[c]]  → "accaccacc"
```

---

## 💡 Approach: Recursion

The solution uses recursion to handle nested brackets.

A shared index `idx` keeps track of the current position in the string.

### Logic

* If the current character is a **digit**, build the complete number.
* If the current character is `[`, make a recursive call to decode the substring inside the brackets.
* If the current character is `]`, return the decoded string of the current recursive call.
* If the current character is a **letter**, append it directly to the result.

---

## 🔄 Flow

For:

```text
3[a2[c]]
```

The recursive flow is:

```text
getDecodedString("3[a2[c]]")
│
├── Read 3 → num = 3
│
├── '[' → Recursive Call
│   │
│   ├── Read 'a' → sb = "a"
│   ├── Read 2 → num = 2
│   │
│   ├── '[' → Recursive Call
│   │   │
│   │   ├── Read 'c' → sb = "c"
│   │   └── ']' → Return "c"
│   │
│   ├── Repeat "c" 2 times
│   ├── sb = "acc"
│   └── ']' → Return "acc"
│
├── Repeat "acc" 3 times
│
└── Result = "accaccacc"
```

---

## 🧪 Dry Run

### Input

```text
s = "3[a2[c]]"
```

| Step | Character | Action                 | `num` | `sb`          |
| ---- | --------- | ---------------------- | ----- | ------------- |
| 1    | `3`       | Build number           | 3     | `""`          |
| 2    | `[`       | Start recursion        | 3     | `""`          |
| 3    | `a`       | Append character       | 0     | `"a"`         |
| 4    | `2`       | Build number           | 2     | `"a"`         |
| 5    | `[`       | Start recursion        | 2     | `"a"`         |
| 6    | `c`       | Append character       | 0     | `"c"`         |
| 7    | `]`       | Return from recursion  | 0     | `"c"`         |
| 8    | —         | Repeat `"c"` 2 times   | 0     | `"acc"`       |
| 9    | `]`       | Return `"acc"`         | 0     | `"acc"`       |
| 10   | —         | Repeat `"acc"` 3 times | 0     | `"accaccacc"` |

### Output

```text
accaccacc
```

---

## 💻 Java Solution

```java
class Solution {
    int idx = 0;

    public String getDecodedString(String s) {
        StringBuilder sb = new StringBuilder();
        int num = 0;

        while (idx < s.length()) {
            char ch = s.charAt(idx);

            if (Character.isDigit(ch)) {
                num = num * 10 + (ch - '0');
                idx++;

            } else if (ch == '[') {
                idx++;

                String str = getDecodedString(s);

                for (int i = 0; i < num; i++) {
                    sb.append(str);
                }

                num = 0;

            } else if (ch == ']') {
                idx++;
                return sb.toString();

            } else {
                sb.append(ch);
                idx++;
            }
        }

        return sb.toString();
    }

    public String decodeString(String s) {
        idx = 0;
        return getDecodedString(s);
    }
}
```

---

## ⏱️ Complexity Analysis

### Time Complexity

```text
O(N)
```

where `N` represents the size of the decoded output, since each output character must be constructed.

### Space Complexity

```text
O(D)
```

for the recursion stack, where `D` is the maximum nesting depth of the encoded string.

Additional space is also required to store the decoded output.

---

## 🔑 Key Concepts

* Recursion
* String Parsing
* StringBuilder
* Nested Structures
* Global Index Tracking
* Multi-digit Number Handling

---

## 📚 Example Test Cases

```text
Input:  "3[a]2[bc]"
Output: "aaabcbc"
```

```text
Input:  "3[a2[c]]"
Output: "accaccacc"
```

```text
Input:  "2[abc]3[cd]ef"
Output: "abcabccdcdcdef"
```

```text
Input:  "10[a]"
Output: "aaaaaaaaaa"
```
