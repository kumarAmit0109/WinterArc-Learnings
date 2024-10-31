# Maximum Number of Moves in a Grid

**Problem Link:** [Maximum Number of Moves in a Grid](https://leetcode.com/problems/maximum-number-of-moves-in-a-grid/)

## Approach 1:

1. **Recursive DFS Function**:
   - The `findMaxMoves` function performs a Depth-First Search (DFS) to explore all possible moves from a given cell `(row, col)` in the grid.
   - It checks the three possible directions (up, right, down) to move to the next column.
2. **Base Case**:

   - If the current column is the last one, return 0 since no more moves are possible.

3. **Direction Iteration**:

   - For each direction (`dRow` can be -1 for up, 0 for right, and 1 for down):
     - Calculate the new cell position `(newRow, newCol)`.
     - Check if `newRow` is within bounds and if the value at the new position is greater than the current position.
     - If valid, recursively call `findMaxMoves` and update `maxCount`.

4. **Starting Point**:
   - The `maxMoves` function initiates the search from every cell in the first column and keeps track of the maximum moves found.

### Time Complexity

- **O(n \* m)**, where \(n\) is the number of rows and \(m\) is the number of columns. Each cell is visited once.

### Space Complexity

- **O(n \* m)**, for the recursion stack in the worst case when all cells are connected.

### Code Implementation

```cpp
class Solution {
public:
    int findMaxMoves(int row, int col, vector<vector<int>>& grid) {
        int rows = grid.size();
        int cols = grid[0].size();

        // Base case: If we are at the last column, no more moves possible
        if (col == cols - 1) return 0;

        int maxCount = 0;

        // Try moving in all three possible directions
        for (int dRow : {-1, 0, 1}) {
            int newRow = row + dRow;
            int newCol = col + 1;

            // Check if within bounds and if the move is to a higher value
            if (newRow >= 0 && newRow < rows && grid[newRow][newCol] > grid[row][col]) {
                maxCount = max(maxCount, 1 + findMaxMoves(newRow, newCol, grid));
            }
        }

        return maxCount;
    }

    int maxMoves(vector<vector<int>>& grid) {
        int rows = grid.size();
        int cols = grid[0].size();
        int maxMoves = 0;

        // Start finding maximum moves from each cell in the first column
        for (int row = 0; row < rows; row++) {
            maxMoves = max(maxMoves, findMaxMoves(row, 0, grid));
        }

        return maxMoves;
    }
};
```

## Approach 2 (Dynamic Programming with Memoization)

### Idea

1. **Recursive DFS with Memoization**:

   - The `findMaxMoves` function performs a Depth-First Search (DFS) to explore all possible moves from a given cell `(row, col)` in the grid while storing results in a memoization table to avoid redundant calculations.

2. **Base Case**:

   - If the current column is the last one, return 0 since no more moves are possible.

3. **Memoization Check**:

   - Before performing calculations, check if the result for the current cell is already computed and stored in the `memo` table. If so, return that stored value.

4. **Direction Iteration**:

   - For each direction (`dRow` can be -1 for up, 0 for right, and 1 for down):
     - Calculate the new cell position `(newRow, newCol)`.
     - Check if `newRow` is within bounds and if the value at the new position is greater than the current position.
     - If valid, recursively call `findMaxMoves` and update `maxCount`.

5. **Store Result**:

   - After processing all possible moves from the current cell, store the maximum count in the memoization table for future reference.

6. **Starting Point**:
   - The `maxMoves` function initiates the search from every cell in the first column and keeps track of the maximum moves found.

### Time Complexity

- **O(n \* m)**, where \(n\) is the number of rows and \(m\) is the number of columns. Each cell is processed once due to memoization.

### Space Complexity

- **O(n \* m)**, for the memoization table used to store results.

### Code Implementation

```cpp
class Solution {
public:
    int findMaxMoves(int row, int col, vector<vector<int>>& grid, vector<vector<int>>& memo) {
        int rows = grid.size();
        int cols = grid[0].size();

        // Base case: If we are at the last column, no more moves possible
        if (col == cols - 1) return 0;

        // If already computed, return the stored result
        if (memo[row][col] != -1) return memo[row][col];

        int maxCount = 0;

        // Try moving in all three possible directions
        for (int dRow : {-1, 0, 1}) {
            int newRow = row + dRow;
            int newCol = col + 1;

            // Check if within bounds and if the move is to a higher value
            if (newRow >= 0 && newRow < rows && grid[newRow][newCol] > grid[row][col]) {
                maxCount = max(maxCount, 1 + findMaxMoves(newRow, newCol, grid, memo));
            }
        }

        // Store result in memoization table
        memo[row][col] = maxCount;
        return maxCount;
    }

    int maxMoves(vector<vector<int>>& grid) {
        int rows = grid.size();
        int cols = grid[0].size();
        int maxMoves = 0;

        // Memoization table to store the max moves from each cell
        vector<vector<int>> memo(rows, vector<int>(cols, -1));

        // Start finding maximum moves from each cell in the first column
        for (int row = 0; row < rows; row++) {
            maxMoves = max(maxMoves, findMaxMoves(row, 0, grid, memo));
        }

        return maxMoves;
    }
};
```

## Approach 3 (Dynamic Programming)

### Idea

1. **Dynamic Programming Table**:

   - The `maxMoves` function uses a 2D DP table `dp` where `dp[row][col]` stores the maximum number of moves possible starting from cell `(row, col)`.

2. **Backward Iteration**:

   - The algorithm iterates from the second-last column back to the first column. This ensures that when processing a cell, all potential moves to the next column have already been calculated.

3. **Direction Iteration**:

   - For each cell, check the three possible moves to the next column:
     - Up `(row - 1, col + 1)`
     - Straight `(row, col + 1)`
     - Down `(row + 1, col + 1)`
   - Calculate the new position based on `dRow` and ensure it's within bounds and that the move is to a higher value.

4. **Updating DP Table**:

   - If the move is valid, update the DP table by calculating the maximum moves possible from the current cell using previously computed values.

5. **Calculate Maximum Moves**:
   - After filling in the DP table, the function computes the maximum number of moves starting from any cell in the first column.

### Time Complexity

- **O(n \* m)**, where \(n\) is the number of rows and \(m\) is the number of columns. Each cell is processed once.

### Space Complexity

- **O(n \* m)**, for the DP table used to store results.

### Code Implementation

```cpp
class Solution {
public:
    int maxMoves(vector<vector<int>>& grid) {
        int rows = grid.size();
        int cols = grid[0].size();

        // DP table where dp[row][col] stores the max moves possible from (row, col)
        vector<vector<int>> dp(rows, vector<int>(cols, 0));

        // Iterate from the second-last column back to the first column
        for (int col = cols - 2; col >= 0; col--) {
            for (int row = 0; row < rows; row++) {

                // Check three possible moves: (row - 1, col + 1), (row, col + 1), and (row + 1, col + 1)
                for (int dRow : {-1, 0, 1}) {
                    int newRow = row + dRow;
                    int newCol = col + 1;

                    // Ensure newRow is within bounds and the move is to a higher value
                    if (newRow >= 0 && newRow < rows && grid[newRow][newCol] > grid[row][col]) {
                        dp[row][col] = max(dp[row][col], 1 + dp[newRow][newCol]);
                    }
                }
            }
        }

        // Calculate the maximum moves starting from any cell in the first column
        int maxMoves = 0;
        for (int row = 0; row < rows; row++) {
            maxMoves = max(maxMoves, dp[row][0]);
        }

        return maxMoves;
    }
};
```
