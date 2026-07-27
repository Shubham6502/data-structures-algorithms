# LRU Cache

**Difficulty:** Medium  
**Topic:** Design, HashMap, LinkedHashMap

---

## Problem Statement

Design a data structure that follows the **Least Recently Used (LRU)** cache policy.

Implement the following operations:

- `LRUCache(int capacity)` → Initialize the cache with a fixed capacity.
- `get(int key)` → Return the value if the key exists; otherwise return `-1`.
- `put(int key, int value)` → Insert or update the key. If the cache is full, remove the **least recently used** item before inserting.

Both operations should work efficiently.

---

## What is LRU?

**LRU (Least Recently Used)** means the item that has **not been accessed for the longest time** will be removed first.

Example (Capacity = 3)

```
put(1,10)
put(2,20)
put(3,30)

Cache:
1 2 3
```

Now,

```
get(2)
```

Since key `2` was recently used, it moves to the end.

```
Cache:
1 3 2
```

Now,

```
put(4,40)
```

The cache is full.

The least recently used key is **1**, so remove it.

```
Cache:
3 2 4
```

---

## Why LinkedHashMap?

`LinkedHashMap` maintains the insertion order of elements.

If we create it like this:

```java
new LinkedHashMap<>(capacity, 0.75f, true);
```

the last parameter `true` enables **access-order**.

This means whenever we call:

```java
map.get(key);
```

or

```java
map.put(existingKey, value);
```

that key becomes the **most recently used** and automatically moves to the end.

Example:

```
Initial

1 2 3
```

After

```
get(2)
```

```
1 3 2
```

No extra code is needed.

---

## Approach

### 1. Initialize LinkedHashMap

```java
map = new LinkedHashMap<>(capacity, 0.75f, true);
```

The third argument (`true`) automatically maintains access order.

---

### 2. get(key)

If the key doesn't exist:

```
return -1
```

Otherwise,

```
return map.get(key)
```

Calling `get()` automatically moves the key to the end.

---

### 3. put(key, value)

#### Case 1: Key already exists

Update the value.

```
map.put(key, value);
```

Since access-order is enabled, it automatically becomes the most recently used.

---

#### Case 2: Cache is full

Remove the first element.

The first element is always the least recently used.

```java
int first = map.entrySet().iterator().next().getKey();
map.remove(first);
```

---

#### Case 3: Insert new key

```java
map.put(key, value);
```

The new key becomes the most recently used.

---

## Dry Run

### Capacity = 2

### put(1,1)

```
1
```

---

### put(2,2)

```
1 2
```

---

### get(1)

Return

```
1
```

Cache becomes

```
2 1
```

because key `1` is recently used.

---

### put(3,3)

Cache is full.

Least recently used = `2`

Remove it.

Insert `3`.

```
1 3
```

---

### get(2)

Key doesn't exist.

```
-1
```

---

### put(4,4)

Cache before:

```
1 3
```

Least recently used = `1`

Remove it.

Insert `4`.

```
3 4
```

---

## Code

```java
class LRUCache {

    LinkedHashMap<Integer, Integer> map;
    int capacity;

    public LRUCache(int capacity) {
        this.capacity = capacity;
        map = new LinkedHashMap<>(capacity, 0.75f, true);
    }

    public int get(int key) {
        if (!map.containsKey(key))
            return -1;

        return map.get(key);
    }

    public void put(int key, int value) {

        if (map.containsKey(key)) {
            map.put(key, value);
            return;
        }

        if (map.size() == capacity) {
            int first = map.entrySet().iterator().next().getKey();
            map.remove(first);
        }

        map.put(key, value);
    }
}
```

---

## Complexity Analysis

### Time Complexity

| Operation | Complexity |
|-----------|------------|
| `get()` | **O(1)** |
| `put()` | **O(1)** |

---

### Space Complexity

The cache stores at most **capacity** elements.

```
O(capacity)
```

---

## Key Concepts

### Access Order

```java
new LinkedHashMap<>(capacity, 0.75f, true);
```

The third parameter (`true`) enables **access-order**, meaning recently accessed entries move to the end.

---

### Finding the LRU Item

The first entry in the `LinkedHashMap` is always the least recently used.

```java
int first = map.entrySet().iterator().next().getKey();
```

Explanation:

```
map.entrySet()
        ↓
All key-value pairs

iterator()
        ↓
Iterator

next()
        ↓
First key-value pair

getKey()
        ↓
Least Recently Used key
```

---

## Interview Tips

- `LinkedHashMap` can implement an LRU cache with very little code.
- Setting the constructor's third parameter to `true` enables automatic access-order maintenance.
- Updating an existing key with `put()` also marks it as recently used.
- When the cache reaches capacity, remove the first entry before inserting a new one.
- A common interview follow-up is implementing the same cache **without `LinkedHashMap`**, using a **HashMap + Doubly Linked List** for O(1) operations.

---

## Similar Problems

- LFU Cache
- Design HashMap
- Design Browser History
- Insert Delete GetRandom O(1)
```
