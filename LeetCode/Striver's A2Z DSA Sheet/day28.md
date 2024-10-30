# 136. Rat in a Maze

### Problem Link

[Rat in a Maze - Naukri](https://www.naukri.com/code360/problems/rat-in-a-maze_1215030)

## Approach: Backtracking with Lexicographical Order

### Algorithm

1. **Base Case**: If we reach the destination cell `(n-1, n-1)`, add the path to the result.
2. **Recursive Calls**:
   - Mark the current cell as visited to avoid cycles.
   - Attempt moves in lexicographical order (Down, Left, Right, Up).
   - For each move, check bounds, cell visit status, and cell accessibility.
3. **Backtracking**:
   - Unmark the current cell to allow other paths to explore this cell.

### Time Complexity

- \(O(4^{n^2})\): Exploring all paths through backtracking, with four directions at each step.

### Space Complexity

- \(O(n^2)\): For the visited matrix.

### Code

```cpp
#include <bits/stdc++.h>
using namespace std;

void findPaths(int row, int col, vector<vector<int>>& arr, int n, string path, vector<string>& result, vector<vector<int>>& visited) {
    // Base case: if we reach the destination cell (n-1, n-1)
    if (row == n - 1 && col == n - 1) {
        result.push_back(path);
        return;
    }

    // Mark the current cell as visited
    visited[row][col] = 1;

    // Explore all four directions in lexicographical order: Down, Left, Right, Up

    // Move Down
    if (row + 1 < n && !visited[row + 1][col] && arr[row + 1][col] == 1) {
        findPaths(row + 1, col, arr, n, path + "D", result, visited);
    }

    // Move Left
    if (col - 1 >= 0 && !visited[row][col - 1] && arr[row][col - 1] == 1) {
        findPaths(row, col - 1, arr, n, path + "L", result, visited);
    }

    // Move Right
    if (col + 1 < n && !visited[row][col + 1] && arr[row][col + 1] == 1) {
        findPaths(row, col + 1, arr, n, path + "R", result, visited);
    }

    // Move Up
    if (row - 1 >= 0 && !visited[row - 1][col] && arr[row - 1][col] == 1) {
        findPaths(row - 1, col, arr, n, path + "U", result, visited);
    }

    // Unmark the current cell as visited (backtracking)
    visited[row][col] = 0;
}

vector<string> searchMaze(vector<vector<int>>& arr, int n) {
    vector<string> result;
    vector<vector<int>> visited(n, vector<int>(n, 0));

    // Start the search from (0, 0) if the start cell is not blocked
    if (arr[0][0] == 1) {
        findPaths(0, 0, arr, n, "", result, visited);
    }


    return result;
}
```
