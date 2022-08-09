# Flood Fill

Page on leetcode:https://leetcode.com/problems/flood-fill/

## Problem Statement

An image is represented by an m x n integer grid image where image[i][j] represents the pixel value of the image.

You are also given three integers sr, sc, and color. You should perform a flood fill on the image starting from the pixel image[sr][sc].

To perform a flood fill, consider the starting pixel, plus any pixels connected 4-directionally to the starting pixel of the same color as the starting pixel, plus any pixels connected 4-directionally to those pixels (also with the same color), and so on. Replace the color of all of the aforementioned pixels with color.

Return the modified image after performing the flood fill.

### Constraints

- m == image.length
- n == image[i].length
- 1 <= m, n <= 50
- 0 <= image[i][j], color < 216
- 0 <= sr < m
- 0 <= sc < n

### Example

```
Input: image = [[1,1,1],[1,1,0],[1,0,1]], sr = 1, sc = 1, color = 2
Output: [[2,2,2],[2,2,0],[2,0,1]]
Explanation: From the center of the image with position (sr, sc) = (1, 1) (i.e., the red pixel), all pixels connected by a path of the same color as the starting pixel (i.e., the blue pixels) are colored with the new color.
Note the bottom corner is not colored 2, because it is not 4-directionally connected to the starting pixel.
```

```
Input: image = [[0,0,0],[0,0,0]], sr = 0, sc = 0, color = 0
Output: [[0,0,0],[0,0,0]]
Explanation: The starting pixel is already colored 0, so no changes are made to the image.
```

## Solution

### Pseudocode

1. If starting pixel current color === new color, return image
2. If not change current pixel to new color
3. Check if left pixel is in bounds and current color, if true change color
4. If false return
5. Check if top pixel is in bounds and current color
6. Recursion...

### Initial Solution

This solution has a time complexity O(n) with a space complexity O(n)

```javascript
const floodFill = function (image, sr, sc, newColor) {
  if (image[sr][sc] === newColor) return image;
  const currentColor = image[sr][sc];

  fillPixel(sr, sc);

  function fillPixel(sr, sc) {
    if (
      sr < 0 ||
      sr > image.length - 1 ||
      sc < 0 ||
      sc > image[0].length - 1 ||
      image[sr][sc] !== currentColor
    ) {
      return;
    }
    image[sr][sc] = newColor;
    fillPixel(sr, sc - 1);
    fillPixel(sr - 1, sc);
    fillPixel(sr, sc + 1);
    fillPixel(sr + 1, sc);
  }
  return image;
};
```

### Alternative Solution

This solution also has time complexity of O(n) and space complexity of O(n). This is taking an iterative DFS approach instead of the recursive DFS approach above. You can see an explanation of the recursive solution here: https://www.youtube.com/watch?v=aehEcTEPtCs

```javascript
const floodFill = function (image, sr, sc, newColor) {
  const currentColor = image[sr][sc];
  if (currentColor !== newColor) {
    const stack = [[sr, sc]];
    while (stack.length > 0) {
      [sr, sc] = stack.pop();
      if (
        !(
          sr < 0 ||
          sr > image.length - 1 ||
          sc < 0 ||
          sc > image[0].length - 1 ||
          image[sr][sc] !== currentColor
        )
      ) {
        image[sr][sc] = newColor;
        stack.push([sr, sc - 1], [sr - 1, sc], [sr, sc + 1], [sr + 1, sc]);
      }
    }
  }
  return image;
};
```
