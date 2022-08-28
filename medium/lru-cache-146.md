# LRU Cache

Page on leetcode: https://leetcode.com/problems/lru-cache/

## Problem Statement

Design a data structure that follows the constraints of a Least Recently Used (LRU) cache.

Implement the LRUCache class:

- LRUCache(int capacity) Initialize the LRU cache with positive size capacity.
- int get(int key) Return the value of the key if the key exists, otherwise return -1.
- void put(int key, int value) Update the value of the key if the key exists. Otherwise, add the key-value pair to the cache. If the number of keys exceeds the capacity from this operation, evict the least recently used key.

The functions get and put must each run in O(1) average time complexity.

### Constraints

- 1 <= capacity <= 3000
- 0 <= key <= 104
- 0 <= value <= 105
- At most 2 \* 105 calls will be made to get and put.

### Example

```
Input
["LRUCache", "put", "put", "get", "put", "get", "put", "get", "get", "get"]
[[2], [1, 1], [2, 2], [1], [3, 3], [2], [4, 4], [1], [3], [4]]
Output
[null, null, null, 1, null, -1, null, -1, 3, 4]

Explanation
LRUCache lRUCache = new LRUCache(2);
lRUCache.put(1, 1); // cache is {1=1}
lRUCache.put(2, 2); // cache is {1=1, 2=2}
lRUCache.get(1);    // return 1
lRUCache.put(3, 3); // LRU key was 2, evicts key 2, cache is {1=1, 3=3}
lRUCache.get(2);    // returns -1 (not found)
lRUCache.put(4, 4); // LRU key was 1, evicts key 1, cache is {4=4, 3=3}
lRUCache.get(1);    // return -1 (not found)
lRUCache.get(3);    // return 3
lRUCache.get(4);    // return 4
```

## Solution

### Initial Thoughts

- get and put time complexity of O(1) suggest hashmap and maybe a stack
- least recently used suggest some sort, based on a counter
- If I used purely a stack put would be O(1), get would be O(n)

### Optimized Solution

You can see an explanation of the solution here: https://www.youtube.com/watch?v=7ABFKPK2hD4

#### Psuedocode

1. Create a Node class for creating a doubly linked list
2. Create LRUCache class with two dummy nodes (old and recent), the capacity and a hashmap for the cache
3. Create get method, removes then inserts node from list and returns if it exist, otherwise returns -1
4. Create put method, removes node from list if exist, updates cache value, inserts into list

- If cache size is too big, removes oldest node from list and deletes from cache

5. Create remove helper method, takes out a node from list and reconnects adjacent pointers
6. Create insert helper method, inserts node between "recent" dummy node and next node in list

```javascript
const Node = function (key, val) {
  this.key = key;
  this.val = val;
  this.prev = null;
  this.next = null;
};

const LRUCache = function (capacity) {
  this.cap = capacity;
  this.cache = new Map();
  this.old = new Node('old', 0);
  this.recent = new Node('recent', 0);
  this.old.next = this.recent;
  this.recent.prev = this.old;
};

LRUCache.prototype.remove = function (node) {
  prev = node.prev;
  nxt = node.next;
  prev.next = nxt;
  nxt.prev = prev;
};

LRUCache.prototype.insert = function (node) {
  prev = this.recent.prev;
  nxt = this.recent;
  prev.next = node;
  nxt.prev = node;
  node.next = nxt;
  node.prev = prev;
};

LRUCache.prototype.get = function (key) {
  if (this.cache.has(key)) {
    this.remove(this.cache.get(key));
    this.insert(this.cache.get(key));
    return this.cache.get(key).val;
  }
  return -1;
};

LRUCache.prototype.put = function (key, value) {
  if (this.cache.has(key)) {
    this.remove(this.cache.get(key));
  }
  this.cache.set(key, new Node(key, value));
  this.insert(this.cache.get(key));

  if (this.cache.size > this.cap) {
    const lru = this.old.next;
    this.remove(lru);
    this.cache.delete(lru.key);
  }
};
```
