# Course Schedule

Page on leetcode: https://leetcode.com/problems/course-schedule/

## Problem Statement

There are a total of numCourses courses you have to take, labeled from 0 to numCourses - 1. You are given an array prerequisites where prerequisites[i] = [ai, bi] indicates that you must take course bi first if you want to take course ai.

- For example, the pair [0, 1], indicates that to take course 0 you have to first take course 1.

Return true if you can finish all courses. Otherwise, return false.

### Constraints

- 1 <= numCourses <= 2000
- 0 <= prerequisites.length <= 5000
- prerequisites[i].length == 2
- 0 <= ai, bi < numCourses
- All the pairs prerequisites[i] are unique.

### Example

```
Input: numCourses = 2, prerequisites = [[1,0]]
Output: true
Explanation: There are a total of 2 courses to take.
To take course 1 you should have finished course 0. So it is possible.
```

```
Input: numCourses = 2, prerequisites = [[1,0],[0,1]]
Output: false
Explanation: There are a total of 2 courses to take.
To take course 1 you should have finished course 0, and to take course 0 you should also have finished course 1. So it is impossible.
```

## Solution

- If courses form a loop it's not possible
- What if there are no prereq's? Does that automatically mean true?
- Could we store in hashmap and see if we find a loop?
- Can a course have multiple pre-reqs?

### Pseudocode

1. Create a map
2. Iterate thru array
3. If prereq isn't in map, and course isn't in map, add to map
4. If prereq is in map and prereq value is current course return false
5. If you go thru whole array then return true

### Initial Attempt

```javascript
const canFinish = function (numCourses, prerequisites) {
  const map = new Map();

  for (let course of prerequisites) {
    const [currCourse, prereq] = course;
    //  Courses create a loop
    if (map.has(prereq) && map.get(prereq) === currCourse) {
      return false;
    } else if (currCourse === prereq) {
      return false;
    }

    map.set(currCourse, prereq);
  }

  return true;
};
```

### Optimized Solution

The time complexity of this approach is O(n + p) and space complexity of O(n). You can see an explanation of the solution here: https://www.youtube.com/watch?v=EgI5nU9etnU

There is a good video that explains Topological Sort here: https://www.youtube.com/watch?v=eL-KzMXSXXI

```javascript
const canFinish = function (numCourses, prerequisites) {
  // Create an adjacency list with empty arrays
  const map = {};
  for (let i = 0; i < numCourses; i++) {
    map[i] = [];
  }
  // Add prereqs to adjacency list
  for (let j = 0; j < prerequisites.length; j++) {
    const [course, prereq] = prerequisites[j];
    map[course].push(prereq);
  }

  // Set for checking if we've seen a node on a DFS path. If so we are in a loop.
  const visited = new Set();

  // Recursive helper function
  function dfs(course) {
    if (visited.has(course)) {
      return false;
    }
    if (map[course] === []) {
      return true;
    }
    visited.add(course);
    // Check all prereqs of this course
    for (prereq of map[course]) {
      if (!dfs(prereq)) {
        return false;
      }
    }
    // remove course from visited and set prereqs to empty if they were all true. Helps speed up algorithm to not do double checking.
    visited.delete(course);
    map[course] = [];
    return true;
  }

  // Check all courses in case the graph is not connected.
  for (let k = 0; k < numCourses; k++) {
    if (!dfs(k)) {
      return false;
    }
  }

  return true;
};
```
