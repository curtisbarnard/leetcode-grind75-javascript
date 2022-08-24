# Robot Bounded In Circle

Page on leetcode: https://leetcode.com/problems/robot-bounded-in-circle/

## Problem Statement

On an infinite plane, a robot initially stands at (0, 0) and faces north. Note that:

- The north direction is the positive direction of the y-axis.
- The south direction is the negative direction of the y-axis.
- The east direction is the positive direction of the x-axis.
- The west direction is the negative direction of the x-axis.

The robot can receive one of three instructions:

- "G": go straight 1 unit.
- "L": turn 90 degrees to the left (i.e., anti-clockwise direction).
- "R": turn 90 degrees to the right (i.e., clockwise direction).

The robot performs the instructions given in order, and repeats them forever.

Return true if and only if there exists a circle in the plane such that the robot never leaves the circle.

### Constraints

- 1 <= instructions.length <= 100
- instructions[i] is 'G', 'L' or, 'R'.

### Example

```
Input: instructions = "GGLLGG"
Output: true
Explanation: The robot is initially at (0, 0) facing the north direction.
"G": move one step. Position: (0, 1). Direction: North.
"G": move one step. Position: (0, 2). Direction: North.
"L": turn 90 degrees anti-clockwise. Position: (0, 2). Direction: West.
"L": turn 90 degrees anti-clockwise. Position: (0, 2). Direction: South.
"G": move one step. Position: (0, 1). Direction: South.
"G": move one step. Position: (0, 0). Direction: South.
Repeating the instructions, the robot goes into the cycle: (0, 0) --> (0, 1) --> (0, 2) --> (0, 1) --> (0, 0).
Based on that, we return true.
```

## Solution

- Can I cancel out the letters in a systematic way?
- Other option is iterate thru string and update a location
  - if final location is (0,0) it's true
- Vector addition?
- If final direction is north and not loc (0,0) false

### Pseudocode

1. Create direction array
2. Create current dir
3. Create current loc
4. Iterate thru string, update dir and loc
5. If cur loc != 0,0 and cur dir = north return false

### Initial Attempt

```javascript
const isRobotBounded = function (instructions) {
  const dir = ['N', 'E', 'S', 'W'];
  const dirMove = {
    N: [0, 1],
    E: [1, 0],
    S: [0, -1],
    W: [-1, 0],
  };
  let curDir = 'N';
  const curLoc = [0, 0];

  for (i in instructions) {
    if (i === 'L') {
    } else if (i === 'R') {
    } else {
      curLoc = [curLoc[0] + dirMove[curDir][0], curLoc[1] + dirMove[curDir][1]];
    }
  }
};
```

### Optimized Solution

Time complexity for this problem is O(n) and space complexity is O(1). You can see an explanation of the solution here: https://www.youtube.com/watch?v=nKv2LnC_g6E

```javascript
const isRobotBounded = function (instructions) {
  let [dirX, dirY] = [0, 1];
  let [x, y] = [0, 0];

  for (let i of instructions) {
    if (i === 'L') {
      [dirX, dirY] = [-dirY, dirX];
    } else if (i === 'R') {
      [dirX, dirY] = [dirY, -dirX];
    } else {
      [x, y] = [x + dirX, y + dirY];
    }
  }

  return (x === 0 && y === 0) || dirY !== 1;
};
```
