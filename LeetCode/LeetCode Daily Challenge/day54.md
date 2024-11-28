# Rotating the Box

**Problem Link**: [Rotating the Box](https://leetcode.com/problems/rotating-the-box/description/)

## Approach 1: Rotate and Apply Gravity

### Idea

1. Rotate the box 90 degrees clockwise to create a new grid.
2. Apply gravity to the rotated grid:
   - Stones (`#`) fall to the lowest possible position unless blocked by an obstacle (`*`).

### Algorithm

1. **Rotation**:
   - Create a new grid where the rows of the original box become the columns of the rotated box.
   - Fill the new grid by iterating through the original box and mapping positions.
2. **Gravity**:
   - For each column in the rotated grid:
     - Use a pointer (`end`) to track the lowest available position for a stone.
     - Move stones (`#`) down, stopping at obstacles (`*`).

### Time Complexity

- **O(m \* n)**: Iterate through all cells for rotation and gravity.

### Space Complexity

- **O(m \* n)**: Additional space for the rotated grid.

### Code

```cpp
class Solution {
public:
    vector<vector<char>> rotateTheBox(vector<vector<char>>& box) {
        int m = box.size(), n = box[0].size();

        // Step 1: Rotate the box 90 degrees clockwise
        vector<vector<char>> rotatedBox(n, vector<char>(m, '.'));
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                rotatedBox[j][m - 1 - i] = box[i][j];
            }
        }

        // Step 2: Apply gravity to the stones in the rotated box
        for (int col = 0; col < rotatedBox[0].size(); ++col) {
            int end = rotatedBox.size() - 1; // Pointer to the bottom-most position
            for (int row = rotatedBox.size() - 1; row >= 0; --row) {
                if (rotatedBox[row][col] == '*') {
                    // Encounter obstacle, move end pointer above it
                    end = row - 1;
                } else if (rotatedBox[row][col] == '#') {
                    // Stone found, swap it with the end pointer if it's empty
                    swap(rotatedBox[row][col], rotatedBox[end][col]);
                    --end;
                }
            }
        }

        return rotatedBox;
    }
};
```

## Approach 2: Transpose, Reverse, and Apply Gravity

### Idea

1. **Transpose the Box**: Convert rows into columns.
2. **Rotate 90 Degrees**: Reverse each row of the transposed grid to achieve the clockwise rotation.
3. **Apply Gravity**:
   - For each column, let the stones (`#`) fall to the lowest available positions, stopping at obstacles (`*`).

### Algorithm

1. **Transpose**:
   - Create a new grid where `result[i][j] = box[j][i]`.
2. **Rotate 90 Degrees**:
   - Reverse each row of the transposed grid.
3. **Apply Gravity**:
   - For each column:
     - Track the lowest available position (`spaceBottomRow`).
     - Move stones (`#`) to the lowest valid position, leaving empty spaces (`.`) behind.

### Time Complexity

- **O(m \* n)**: Processing for transpose, reverse, and applying gravity.

### Space Complexity

- **O(m \* n)**: Additional space for the rotated grid.

### Code

```cpp
class Solution {
public:
    vector<vector<char>> rotateTheBox(vector<vector<char>>& box) {
        int m = box.size();
        int n = box[0].size();

        vector<vector<char>> result(n, vector<char>(m));

        // Transpose the grid
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                result[i][j] = box[j][i];
            }
        }

        // Rotate 90 degrees by reversing each row
        for (vector<char>& row : result) {
            reverse(begin(row), end(row));
        }

        // Apply gravity to stones
        for (int j = 0; j < m; j++) {
            int spaceBottomRow = n - 1; // Pointer for the lowest available position
            for (int i = n - 1; i >= 0; i--) {
                if (result[i][j] == '*') {
                    spaceBottomRow = i - 1; // Obstacle, adjust pointer
                } else if (result[i][j] == '#') {
                    result[i][j] = '.';               // Remove stone from current position
                    result[spaceBottomRow][j] = '#'; // Place stone at the lowest position
                    spaceBottomRow--;               // Update pointer
                }
            }
        }

        return result;
    }
};
```
