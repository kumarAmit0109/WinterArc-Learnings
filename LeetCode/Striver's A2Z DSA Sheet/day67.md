# 296. Rotting Oranges

## Problem Link

[LeetCode - Rotting Oranges](https://leetcode.com/problems/rotting-oranges/)

## Approach: Breadth-First Search (BFS)

### Idea/Intuition

- Model the problem as a multi-source BFS:
  - Start with all rotten oranges (sources) simultaneously.
  - Use BFS to simulate the spread of rot to adjacent fresh oranges in each unit of time.
- If all fresh oranges are rotten after the BFS completes, return the time taken.
- If there are still fresh oranges left, return `-1`.

### Algorithm

1. **Initialization**:
   - Use a queue to store the coordinates of all initially rotten oranges.
   - Count the total number of fresh oranges.
   - Track visited cells to avoid revisiting.
2. **BFS Traversal**:
   - For each rotten orange, try to rot its adjacent fresh oranges (up, down, left, right).
   - Decrease the count of fresh oranges when a new orange becomes rotten.
   - Increment the time whenever at least one orange becomes rotten during a BFS level.
3. **Check Remaining Fresh Oranges**:
   - If there are still fresh oranges left, return `-1`.
   - Otherwise, return the time taken.

### Time Complexity

- **O(M × N)**:
  - Each cell is processed at most once during BFS.

### Space Complexity

- **O(M × N)**:
  - For the `visited` array and the BFS queue.

### Implementation

```cpp
class Solution {
public:
    bool isPossible(int x, int y, vector<vector<int>>& grid, vector<vector<int>>& visited, int m, int n) {
        return (x >= 0 && x < m && y >= 0 && y < n && grid[x][y] == 1 && visited[x][y] == 0);
    }

    int orangesRotting(vector<vector<int>>& grid) {
        int m = grid.size();
        int n = grid[0].size();
        vector<vector<int>> visited(m, vector<int>(n, 0));

        queue<pair<int, int>> que;
        int freshCount = 0;

        // Add all rotten oranges to the queue and count fresh oranges
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == 2) {
                    que.push({i, j});
                    visited[i][j] = 1;
                } else if (grid[i][j] == 1) {
                    freshCount++;
                }
            }
        }

        int time = 0;
        vector<pair<int, int>> directions = {{-1, 0}, {1, 0}, {0, -1}, {0, 1}}; // Up, Down, Left, Right

        while (!que.empty()) {
            int size = que.size();
            bool flag = false;

            for (int k = 0; k < size; k++) {
                auto topNode = que.front();
                que.pop();
                int x = topNode.first;
                int y = topNode.second;

                for (auto dir : directions) {
                    int newX = x + dir.first;
                    int newY = y + dir.second;

                    if (isPossible(newX, newY, grid, visited, m, n)) {
                        flag = true;
                        visited[newX][newY] = 1;
                        que.push({newX, newY});
                        freshCount--;
                    }
                }
            }

            if (flag) {
                time++;
            }
        }

        // If there are still fresh oranges, return -1
        return freshCount == 0 ? time : -1;
    }
};
```

# 297. Flood Fill

## Problem Link

[LeetCode - Flood Fill](https://leetcode.com/problems/flood-fill/description/)

## Approach 1: BFS-Based Flood Fill

### Idea/Intuition

- Use Breadth-First Search (BFS) to change the color of all connected cells with the same initial color as the starting cell (`sr`, `sc`).
- Maintain a `visited` matrix to track which cells have been processed.
- Update the color of the cells as they are processed.

### Algorithm

1. **Initialization**:
   - Get the initial color of the starting cell.
   - If the initial color is the same as the new color, return the original image (no changes needed).
   - Initialize a `visited` matrix to track processed cells.
   - Use a queue to implement BFS and add the starting cell to the queue.
2. **BFS Traversal**:
   - For each cell, explore its neighbors in four directions (up, down, left, right).
   - If a neighbor has the same initial color and hasn't been visited:
     - Add it to the queue.
     - Update its color and mark it as visited.
3. **Return the Updated Image**:
   - Once all reachable cells have been processed, return the modified image.

### Time Complexity

- **O(M × N)**:
  - Each cell in the grid is processed at most once.

### Space Complexity

- **O(M × N)**:
  - For the `visited` matrix and BFS queue.

### Implementation

```cpp
class Solution {
public:
    bool isPossible(int x, int y, vector<vector<int>>& image, vector<vector<int>>& visited, int m, int n, int initialColor) {
        return (x >= 0 && x < m && y >= 0 && y < n && image[x][y] == initialColor && visited[x][y] == 0);
    }

    vector<vector<int>> floodFill(vector<vector<int>>& image, int sr, int sc, int color) {
        int m = image.size();
        int n = image[0].size();
        int initialColor = image[sr][sc];

        // If the initial color is the same as the new color, no changes are needed.
        if (initialColor == color) return image;

        vector<vector<int>> visited(m, vector<int>(n, 0));
        queue<pair<int, int>> que;

        vector<pair<int, int>> directions = {{-1, 0}, {1, 0}, {0, -1}, {0, 1}}; // Up, Down, Left, Right

        que.push({sr, sc});
        visited[sr][sc] = 1;
        image[sr][sc] = color; // Update the starting cell's color.

        while (!que.empty()) {
            pair<int, int> topNode = que.front();
            que.pop();

            int x = topNode.first;
            int y = topNode.second;

            for (auto dir : directions) {
                int newX = x + dir.first;
                int newY = y + dir.second;
                if (isPossible(newX, newY, image, visited, m, n, initialColor)) {
                    que.push({newX, newY});
                    image[newX][newY] = color; // Update color.
                    visited[newX][newY] = 1;   // Mark as visited.
                }
            }
        }
        return image;
    }
};
```

## Approach 2: DFS-Based Flood Fill

### Idea/Intuition

- Use Depth-First Search (DFS) to recursively explore and update the color of all connected cells with the same initial color as the starting cell (`sr`, `sc`).
- A `visited` matrix is used to avoid revisiting cells.

### Algorithm

1. **Initialization**:
   - Determine the dimensions of the image (`m`, `n`).
   - Extract the initial color of the starting cell.
   - If the initial color matches the new color, return the image as-is (no changes needed).
   - Prepare a `visited` matrix to track processed cells.
2. **DFS Traversal**:
   - Define a recursive `dfs` function to:
     - Check boundary conditions and validity (e.g., the cell must match the initial color and not be visited).
     - Mark the current cell as visited and update its color.
     - Recursively call `dfs` on its neighbors (up, down, left, right).
3. **Start DFS**:
   - Call `dfs` from the starting cell and update all connected cells.
4. **Return the Updated Image**:
   - Once all reachable cells are processed, return the modified image.

### Time Complexity

- **O(M × N)**:
  - Each cell is visited at most once.

### Space Complexity

- **O(M × N)**:
  - For the `visited` matrix and recursive stack.

### Implementation

```cpp
class Solution {
public:
    void dfs(int x, int y, vector<vector<int>>& image, vector<vector<int>>& visited, int m, int n, int initialColor, int newColor) {
        // Boundary and validity check
        if (x < 0 || x >= m || y < 0 || y >= n || image[x][y] != initialColor || visited[x][y] == 1) {
            return;
        }

        // Mark the current cell as visited and update its color
        visited[x][y] = 1;
        image[x][y] = newColor;

        // Explore neighbors: Up, Down, Left, Right
        dfs(x - 1, y, image, visited, m, n, initialColor, newColor); // Up
        dfs(x + 1, y, image, visited, m, n, initialColor, newColor); // Down
        dfs(x, y - 1, image, visited, m, n, initialColor, newColor); // Left
        dfs(x, y + 1, image, visited, m, n, initialColor, newColor); // Right
    }

    vector<vector<int>> floodFill(vector<vector<int>>& image, int sr, int sc, int color) {
        int m = image.size();
        int n = image[0].size();
        int initialColor = image[sr][sc];

        // If the initial color is the same as the new color, no changes are needed.
        if (initialColor == color) return image;

        vector<vector<int>> visited(m, vector<int>(n, 0));

        // Start DFS from the starting pixel
        dfs(sr, sc, image, visited, m, n, initialColor, color);

        return image;
    }
};
```

# 298. Detect Cycle in an Undirected Graph

## Problem Link

[GeeksForGeeks - Detect Cycle in an Undirected Graph](https://www.geeksforgeeks.org/problems/detect-cycle-in-an-undirected-graph/1)

## Approach: BFS Traversal

### Idea/Intuition

- Use a BFS traversal to detect cycles in the graph.
- Maintain a `parent` array to track the parent of each node during traversal.
- If a visited node is encountered that is not the parent of the current node, a cycle exists.

### Algorithm

1. **Initialization**:
   - Create a `visited` array to keep track of visited nodes.
   - Use a `parent` array to store the parent of each node.
2. **BFS Traversal**:
   - For every unvisited node, start a BFS.
   - Mark the current node as visited and push it into the queue.
   - For every neighbor of the current node:
     - If unvisited, mark it as visited, set its parent, and push it into the queue.
     - If visited and not the parent of the current node, a cycle is detected.
3. **Cycle Detection**:
   - If the BFS completes without finding any cycles, return `false`.
   - Otherwise, return `true` when a cycle is found.

### Time Complexity

- **O(V + E)**:
  - Each vertex and edge is processed once in BFS.

### Space Complexity

- **O(V)**:
  - For the `visited` and `parent` arrays.

### Implementation

```cpp
class Solution {
  public:
    // Function to detect cycle in an undirected graph.
    bool isCycle(vector<vector<int>>& adj) {
        int n = adj.size(); // Number of nodes
        vector<int> visited(n, 0);
        vector<int> parent(n, -1);

        for (int start = 0; start < n; ++start) {
            if (!visited[start]) {
                queue<int> que;
                que.push(start);
                visited[start] = 1;

                while (!que.empty()) {
                    int node = que.front();
                    que.pop();

                    for (auto neighbor : adj[node]) {
                        if (!visited[neighbor]) {
                            visited[neighbor] = 1;
                            parent[neighbor] = node;
                            que.push(neighbor);
                        } else if (neighbor != parent[node]) {
                            // A visited neighbor that is not the parent indicates a cycle
                            return true;
                        }
                    }
                }
            }
        }

        return false;
    }
};
```
