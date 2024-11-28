# Count Unguarded Cells in the Grid

**Problem Link**: [Count Unguarded Cells in the Grid](https://leetcode.com/problems/count-unguarded-cells-in-the-grid/)

## Approach 1: Checking Guard Visibility for Each Cell

### Idea

1. Use a grid to represent the layout of guards, walls, and unoccupied cells.
2. For each unoccupied cell, check in all four directions (north, east, south, west) to determine if it is visible to a guard.
3. If a guard is found in any direction before encountering a wall, the cell is marked as guarded.

### Algorithm

1. Initialize the grid with '.' for empty cells, 'G' for guards, and 'W' for walls.
2. For each unoccupied cell:
   - Iterate in all four directions to see if a guard can "see" the cell.
   - Stop checking a direction if a wall is encountered.
3. Count the cells that are neither guarded nor occupied.

### Time Complexity

- **O(m _ n _ (m + n))**: For each cell, checking all four directions can take O(m + n) in the worst case.

### Space Complexity

- **O(m \* n)**: Space for the grid.

### Code

```cpp
class Solution {
public:
    int countUnguarded(int m, int n, vector<vector<int>>& guards, vector<vector<int>>& walls) {
        vector<vector<char>> grid(m, vector<char>(n, '.')); // Initialize grid

        // Mark guards and walls
        for (const auto& guard : guards)
            grid[guard[0]][guard[1]] = 'G';
        for (const auto& wall : walls)
            grid[wall[0]][wall[1]] = 'W';

        // Function to check if a cell is guarded
        auto isGuarded = [&](int x, int y) -> bool {
            vector<pair<int, int>> directions = {{-1, 0}, {1, 0}, {0, -1}, {0, 1}};
            for (const auto& dir : directions) {
                int i = x, j = y;
                while (true) {
                    i += dir.first;
                    j += dir.second;
                    if (i < 0 || i >= m || j < 0 || j >= n || grid[i][j] == 'W')
                        break;
                    if (grid[i][j] == 'G')
                        return true; // Guard is visible
                }
            }
            return false; // Not guarded
        };

        int unguardedCount = 0;
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (grid[i][j] == '.' && !isGuarded(i, j)) {
                    ++unguardedCount;
                }
            }
        }

        return unguardedCount;
    }
};
```

## Approach 2: Mark Guarded Cells Directly

### Idea

1. Use a grid to represent guards, walls, and unoccupied cells.
2. For each guard, mark all visible cells in all four directions until a wall or another guard is encountered.
3. Count cells that are still unoccupied and unguarded after marking.

### Algorithm

1. Initialize the grid with '.' for empty cells, 'G' for guards, and 'W' for walls.
2. For each guard:
   - Traverse all four directions, marking cells as guarded ('X').
   - Stop marking in a direction if a wall or another guard is encountered.
3. Count unoccupied cells ('.') in the grid after marking.

### Time Complexity

- **O(m \* n)**: Each cell is visited at most once for marking.

### Space Complexity

- **O(m \* n)**: Space for the grid.

### Code

```cpp
class Solution {
public:
    int countUnguarded(int m, int n, vector<vector<int>>& guards, vector<vector<int>>& walls) {
        vector<vector<char>> grid(m, vector<char>(n, '.')); // Initialize grid

        // Mark guards and walls on the grid
        for (const auto& guard : guards)
            grid[guard[0]][guard[1]] = 'G';
        for (const auto& wall : walls)
            grid[wall[0]][wall[1]] = 'W';

        vector<pair<int, int>> directions = {{-1, 0}, {1, 0}, {0, -1}, {0, 1}};

        // Mark all cells that can be seen by guards
        for (const auto& guard : guards) {
            for (const auto& dir : directions) {
                int x = guard[0], y = guard[1];
                while (true) {
                    x += dir.first;
                    y += dir.second;
                    if (x < 0 || x >= m || y < 0 || y >= n || grid[x][y] == 'G' || grid[x][y] == 'W')
                        break;
                    if (grid[x][y] == '.')
                        grid[x][y] = 'X'; // Mark as guarded
                }
            }
        }

        int unguardedCount = 0;
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (grid[i][j] == '.')
                    ++unguardedCount;
            }
        }

        return unguardedCount;
    }
};
```
