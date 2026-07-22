# Time Based Key-Value Store (LeetCode 981)

## Problem

Design a time-based key-value data structure that can:

- Store multiple values for the same key at different timestamps.
- Retrieve the value associated with the largest timestamp less than or equal to the given timestamp.

---

## Intuition

For every key, we need to store all of its values along with their timestamps.

Example:

```
apple
 ├── (1, "red")
 ├── (4, "green")
 └── (8, "yellow")
```

Notice that **timestamps are given in strictly increasing order**, so each new value can simply be appended to the list.

When retrieving a value:

- If the exact timestamp exists → return it.
- Otherwise return the value with the **largest timestamp smaller than the given timestamp**.

Since timestamps are sorted, **Binary Search** is the perfect choice.

---

# Data Structure

```java
Map<String, List<Pair>> map;
```

Each key maps to a list of `(timestamp, value)` pairs.

Example:

```
{
    "foo" -> [(1,"bar"), (4,"baz"), (7,"abc")],
    "cat" -> [(2,"white"), (6,"black")]
}
```

---

# Pair Class

```java
class Pair{
    int timestamp;
    String value;
}
```

Stores

- timestamp
- corresponding value

Example

```
Pair
-----------------
timestamp = 5
value = "apple"
```

---

# set()

```java
public void set(String key, String value, int timestamp)
```

### Logic

If the key is not present

```
Create a new ArrayList
```

Then append the new pair.

```java
if(!map.containsKey(key)){
    map.put(key,new ArrayList<>());
}

map.get(key).add(new Pair(timestamp,value));
```

---
```java
class TimeMap {
    public class Pair{
        int timestamp;
        String value;
        Pair(int timestamp,String value){
            this.timestamp=timestamp;
            this.value=value;
        }
    }
    Map<String, List<Pair>>map;

    public TimeMap() {
        map=new HashMap();
    }
    
    public void set(String key, String value, int timestamp) {
        if(!map.containsKey(key)){
             map.put(key, new ArrayList<>());
        }
           map.get(key).add(new Pair(timestamp,value) );
       
    }
    
    public String get(String key, int timestamp) {
         List<Pair> list=map.get(key);
            if(list ==null) return "";
         int left=0;
         int right=list.size()-1;
         String result="";
         while(left<=right){
            int mid=(left+right)/2;
        
            if(list.get(mid).timestamp<=timestamp){
                result=list.get(mid).value;
                left=mid+1;
            }else{
                right=mid-1;
            }
         }
         return result;

    }
}

```
## Dry Run

### set("foo","bar",1)

Map before

```
{}
```

Create list

```
foo -> []
```

Add pair

```
foo -> [(1,"bar")]
```

---

### set("foo","baz",4)

Key already exists.

Append directly.

```
foo
|
+---- (1,"bar")
|
+---- (4,"baz")
```

---

### set("foo","abc",7)

```
foo
|
+---- (1,"bar")
|
+---- (4,"baz")
|
+---- (7,"abc")
```

No sorting required because timestamps are already increasing.

---

# get()

```java
public String get(String key, int timestamp)
```

We perform Binary Search on the timestamp list.

Goal:

Find

```
largest timestamp <= given timestamp
```

---

## Why Binary Search?

Suppose

```
foo

1 -> bar
4 -> baz
7 -> abc
10 -> xyz
```

Query

```
get(foo,8)
```

Expected answer

```
abc
```

because

```
7 <= 8
```

and it is the largest valid timestamp.

---

# Binary Search Logic

```java
String result="";

while(left<=right){

    int mid=(left+right)/2;

    if(list.get(mid).timestamp<=timestamp){

        result=list.get(mid).value;
        left=mid+1;

    }else{

        right=mid-1;
    }
}
```

---

## Why move right after finding a valid timestamp?

Suppose

```
timestamps

1 4 7 10
```

Query

```
timestamp = 8
```

---

### First Iteration

```
left = 0
right = 3

mid = 1
```

```
1 4 7 10
  ^
```

```
4 <= 8
```

Possible answer

```
result = baz
```

Move right

```
left = mid + 1
```

because maybe there is a larger valid timestamp.

---

### Second Iteration

```
left = 2
right = 3

mid = 2
```

```
1 4 7 10
    ^
```

```
7 <= 8
```

Better answer

```
result = abc
```

Move right again.

---

### Third Iteration

```
left = 3
right = 3

mid = 3
```

```
1 4 7 10
      ^
```

```
10 > 8
```

Too large.

Move left

```
right = mid - 1
```

Loop ends.

Answer

```
abc
```

---

# Visualization

Query

```
get(foo,8)
```

```
                timestamp = 8

                    ↓

1 -------- 4 -------- 7 -------- 10
|          |          |           |
bar       baz        abc         xyz

Largest timestamp ≤ 8

↓

7

↓

Return "abc"
```

---

# Example Walkthrough

Operations

```java
set("foo","bar",1);
set("foo","baz",4);
set("foo","abc",7);

get("foo",5);
```

Stored

```
foo

(1,bar)

(4,baz)

(7,abc)
```

Binary Search

```
1     4      7

      ^
```

```
4 <= 5

answer = baz
```

Move right

```
7 > 5
```

Stop.

Return

```
baz
```

---

# Edge Cases

## Key doesn't exist

```
get("apple",5)
```

Returns

```
""
```

---

## Timestamp smaller than all timestamps

Stored

```
3
5
7
```

Query

```
1
```

No valid timestamp.

Return

```
""
```

---

## Timestamp larger than every timestamp

Stored

```
1
4
8
```

Query

```
20
```

Return

```
value at timestamp 8
```

---

## Exact timestamp exists

Stored

```
1
4
8
```

Query

```
4
```

Return

```
value at timestamp 4
```

---

# Complexity Analysis

## set()

HashMap lookup

```
O(1)
```

Appending to ArrayList

```
O(1)
```

Overall

```
O(1)
```

---

## get()

Binary Search over timestamps

```
O(log n)
```

where `n` is the number of values stored for that key.

---

## Space Complexity

Each call to `set()` stores one `(timestamp, value)` pair.

```
O(N)
```

where `N` is the total number of stored pairs.

---

# Key Takeaways

- Use a `HashMap` to group values by key.
- Store `(timestamp, value)` pairs in an `ArrayList`.
- Timestamps are already sorted, so insertion is **O(1)**.
- Use **Binary Search** to efficiently find the largest timestamp less than or equal to the query timestamp.
- Keep updating the answer whenever a valid timestamp is found and continue searching to the right for a closer timestamp.
