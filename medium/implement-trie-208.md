# Implement Trie (Prefix Tree)

Page on leetcode: https://leetcode.com/problems/implement-trie-prefix-tree/

## Problem Statement

A trie (pronounced as "try") or prefix tree is a tree data structure used to efficiently store and retrieve keys in a dataset of strings. There are various applications of this data structure, such as autocomplete and spellchecker.

Implement the Trie class:

- Trie() Initializes the trie object.
- void insert(String word) Inserts the string word into the trie.
- boolean search(String word) Returns true if the string word is in the trie (i.e., was inserted before), and false otherwise.
- boolean startsWith(String prefix) Returns true if there is a previously inserted string word that has the prefix prefix, and false otherwise.

### Constraints

- 1 <= word.length, prefix.length <= 2000
- word and prefix consist only of lowercase English letters.
- At most 3 \* 104 calls in total will be made to insert, search, and startsWith.

### Example

```
Input
["Trie", "insert", "search", "search", "startsWith", "insert", "search"]
[[], ["apple"], ["apple"], ["app"], ["app"], ["app"], ["app"]]
Output
[null, null, true, false, true, null, true]

Explanation
Trie trie = new Trie();
trie.insert("apple");
trie.search("apple");   // return True
trie.search("app");     // return False
trie.startsWith("app"); // return True
trie.insert("app");
trie.search("app");     // return True
```

## Solution

- root node val is empty
- need a Node class?
- Node class has list of next letters
- Need some sort of flag for complete word
- search traverse nodes and if it finds a matching node for each letter and last letter is a complete node will return true
- starts with same as search except complete word not necessary

### Pseudocode

1. Create Node class with val, next letters, complete word
2. Trie creates a node with no val, blank next letters and false complete word
3. Insert takes string and puts into array
4. Loops while array is not empty
5. Checks if node contains letter as nextletter, if not add letter and go to it
6. If array is empty then mark current node as complete word
7. Search takes string and puts into array
8. Loops while array is not empty
9. Checks if node contains letter as nextletter, if not return false
10. If array is empty and current node is a complete word return true, else return false
11. StartsWith takes string and puts into array
12. Loops while array is not empty
13. Checks if node contains letter as nextletter, if not return false
14. If array is empty return true

### Initial Solution

I believe this solution has a time complexity of O(n) for insert and search. Space complexity is O(n) for insertion and O(1) for search.

```javascript
const Node = function (val, completeWord) {
  this.val = val;
  this.completeWord = completeWord;
  this.nextLetters = {};
};

const Trie = function () {
  this.root = new Node(null, false);
  return null;
};

Trie.prototype.insert = function (word) {
  const wordArray = word.split('');
  let currChar = this.root;
  while (wordArray.length > 0) {
    const char = wordArray.shift();
    if (char in currChar.nextLetters) {
      currChar = currChar.nextLetters[char];
    } else {
      currChar.nextLetters[char] = new Node(char, false);
      currChar = currChar.nextLetters[char];
    }
  }
  currChar.completeWord = true;
  return null;
};

Trie.prototype.search = function (word) {
  const wordArray = word.split('');
  let currChar = this.root;
  while (wordArray.length > 0) {
    const char = wordArray.shift();
    if (char in currChar.nextLetters) {
      currChar = currChar.nextLetters[char];
    } else {
      return false;
    }
  }
  if (currChar.completeWord) {
    return true;
  }
  return false;
};

Trie.prototype.startsWith = function (prefix) {
  const wordArray = prefix.split('');
  let currChar = this.root;
  while (wordArray.length > 0) {
    const char = wordArray.shift();
    if (char in currChar.nextLetters) {
      currChar = currChar.nextLetters[char];
    } else {
      return false;
    }
  }
  return true;
};
```

### Optimized Solution

The below is roughly the same as above, just with some clean up to make the code more concise. Time is O(n) for insertion and search. Space complexity id O(n) for insertion and O(1) for search.You can see an explanation for this solution here: https://www.youtube.com/watch?v=oobqoCJlHA0

```javascript
const Node = function () {
  this.completeWord = false;
  this.nextLetters = {};
};

const Trie = function () {
  this.root = new Node();
};

Trie.prototype.insert = function (word) {
  let cur = this.root;
  for (char of word) {
    if (!(char in cur.nextLetters)) {
      cur.nextLetters[char] = new Node();
    }
    cur = cur.nextLetters[char];
  }
  cur.completeWord = true;
};

Trie.prototype.search = function (word) {
  let cur = this.root;
  for (char of word) {
    if (!(char in cur.nextLetters)) {
      return false;
    }
    cur = cur.nextLetters[char];
  }
  return cur.completeWord;
};

Trie.prototype.startsWith = function (prefix) {
  let cur = this.root;
  for (char of prefix) {
    if (!(char in cur.nextLetters)) {
      return false;
    }
    cur = cur.nextLetters[char];
  }
  return true;
};
```
