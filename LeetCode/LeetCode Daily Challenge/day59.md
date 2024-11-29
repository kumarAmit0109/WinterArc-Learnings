# Minimum Obstacle Removal to Reach Corner

**Problem Link**: [Minimum Obstacle Removal to Reach Corner](https://leetcode.com/problems/minimum-obstacle-removal-to-reach-corner/description/)

## Approach 1: Dijkstra's Algorithm (Priority Queue)

### Idea

This approach uses **Dijkstra's Algorithm** to determine the minimum obstacles that need to be removed:

- Each cell's obstacle removal count is treated as its "weight."
- A priority queue is used to always process the cell with the smallest obstacle count first.

### Algorithm

1. **Initialization**:

   - Use a priority queue to process cells in increasing order of obstacle removal count.
   - Maintain a `minObstacles` matrix to store the minimum obstacles required to reach each cell.

2. **Dijkstra-like Processing**:

   - Start from the top-left corner and process all possible directions for the current cell.
   - Update the obstacle count for neighboring cells if a better path is found and add them to the priority queue.

3. **Termination**:
   - Return the value at the bottom-right corner in the `minObstacles` matrix, representing the minimum obstacles required.

### Time Complexity

- **O(n × m × log(n × m))**: Each cell is processed at most once, and the priority queue operation takes logarithmic time.

### Space Complexity

- **O(n × m)**: Space required for the `minObstacles` matrix and the priority queue.

---

### Code

```cpp
class Solution {
public:
    #define Cell pair<int, pair<int, int>>
    vector<vector<int>> directions = {{0, 1}, {0, -1}, {1, 0}, {-1, 0}};

    int minimumObstacles(vector<vector<int>>& grid) {
        int rows = grid.size();
        int cols = grid[0].size();

        vector<vector<int>> minObstacles(rows, vector<int>(cols, INT_MAX));
        minObstacles[0][0] = 0;

        priority_queue<Cell, vector<Cell>, greater<Cell>> priorityQueue;
        priorityQueue.push({0, {0, 0}}); // {currentObstacles, {row, col}}

        while (!priorityQueue.empty()) {
            auto currentCell = priorityQueue.top();
            priorityQueue.pop();

            int currentObstacles = currentCell.first;
            int currentRow = currentCell.second.first;
            int currentCol = currentCell.second.second;

            for (auto &direction : directions) {
                int nextRow = currentRow + direction[0];
                int nextCol = currentCol + direction[1];

                if (nextRow < 0 || nextRow >= rows || nextCol < 0 || nextCol >= cols) {
                    continue;
                }

                int obstacle = (grid[nextRow][nextCol] == 1) ? 1 : 0;

                if (currentObstacles + obstacle < minObstacles[nextRow][nextCol]) {
                    minObstacles[nextRow][nextCol] = currentObstacles + obstacle;
                    priorityQueue.push({currentObstacles + obstacle, {nextRow, nextCol}});
                }
            }
        }

        return minObstacles[rows - 1][cols - 1];
    }
};
```

## Approach 2: 0-1 BFS Algorithm

### Idea

This approach uses **0-1 BFS** to solve the problem efficiently:

- Nodes without obstacles are given higher priority by adding them to the front of the deque.
- Nodes with obstacles are added to the back of the deque.

This ensures that the shortest path is explored first while keeping the complexity minimal.

### Algorithm

1. **Initialization**:

   - Use a deque for BFS traversal.
   - Maintain a `minObstacles` matrix to store the minimum obstacles required to reach each cell.

2. **BFS Traversal**:

   - Start from the top-left corner.
   - Explore all possible directions for the current cell.
   - Update the obstacle count for neighboring cells and add them to the deque (front or back based on obstacle presence).

3. **Termination**:
   - Return the value at the bottom-right corner in the `minObstacles` matrix, representing the minimum obstacles required.

### Time Complexity

- **O(n × m)**: Each cell is visited at most once, where `n` and `m` are the dimensions of the grid.

### Space Complexity

- **O(n × m)**: Space required for the `minObstacles` matrix and the deque.

---

### Code

```cpp
class Solution {
public:
    vector<vector<int>> directions = {{0, 1}, {0, -1}, {1, 0}, {-1, 0}};

    int minimumObstacles(vector<vector<int>>& grid) {
        int rows = grid.size();
        int cols = grid[0].size();

        vector<vector<int>> minObstacles(rows, vector<int>(cols, INT_MAX));
        minObstacles[0][0] = 0;

        deque<pair<int, int>> bfsQueue;
        bfsQueue.push_back({0, 0}); // Start at the top-left corner

        while (!bfsQueue.empty()) {
            auto [currentRow, currentCol] = bfsQueue.front();
            bfsQueue.pop_front();

            for (auto &direction : directions) {
                int nextRow = currentRow + direction[0];
                int nextCol = currentCol + direction[1];

                if (nextRow < 0 || nextRow >= rows || nextCol < 0 || nextCol >= cols) {
                    continue;
                }

                int newObstacleCount = minObstacles[currentRow][currentCol] + grid[nextRow][nextCol];

                if (newObstacleCount < minObstacles[nextRow][nextCol]) {
                    minObstacles[nextRow][nextCol] = newObstacleCount;

                    // Push to the front if no obstacle, else push to the back
                    if (grid[nextRow][nextCol] == 0) {
                        bfsQueue.push_front({nextRow, nextCol});
                    } else {
                        bfsQueue.push_back({nextRow, nextCol});
                    }
                }
            }
        }

        return minObstacles[rows - 1][cols - 1];
    }
};
```
