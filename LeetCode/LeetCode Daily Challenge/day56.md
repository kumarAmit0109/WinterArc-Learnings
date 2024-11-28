# Sliding Puzzle

**Problem Link**: [Sliding Puzzle](https://leetcode.com/problems/sliding-puzzle/description/)

## Approach 1: Breadth-First Search (BFS)

### Idea

1. **State Representation**:
   - Represent the board as a string (e.g., `"123450"`), where '0' represents the empty slot.
2. **Valid Moves**:

   - Predefine possible moves for each position in a 2D board:
     - Index 0: Move to 1 or 3
     - Index 1: Move to 0, 2, or 4
     - ...

3. **BFS Traversal**:

   - Start with the initial board state.
   - Explore all possible states by swapping '0' with valid positions.
   - Use a queue to manage states and a set to avoid revisiting.

4. **Goal**:

   - Stop and return the move count when the target state `"123450"` is reached.

5. **Edge Cases**:
   - If the initial state is the target, return 0.

### Algorithm

1. Convert the 2D board to a string.
2. Initialize BFS with the start state.
3. For each state:
   - Find the position of '0'.
   - Try all valid moves, generate new states.
   - If a new state matches the target, return the steps.
   - Otherwise, add it to the queue if not visited.
4. Return -1 if no solution is found.

### Time Complexity

- **O(N! \* N)**:
  - N is the number of board cells (6 in this case).
  - Permutations of states multiplied by state generation.

### Space Complexity

- **O(N! + N)**: Space for the visited set and queue.

### Code

```cpp
class Solution {
public:
    int slidingPuzzle(vector<vector<int>>& board) {
        string target = "123450";

        // Convert the initial board to a string
        string start;
        for (const auto& row : board) {
            for (int num : row) {
                start += to_string(num);
            }
        }

        // If the start is already the target, return 0 moves
        if (start == target) return 0;

        // Define the possible moves from each position
        vector<vector<int>> moves = {
            {1, 3}, {0, 2, 4}, {1, 5}, {0, 4}, {1, 3, 5}, {2, 4}
        };

        // BFS initialization
        queue<pair<string, int>> q;
        unordered_set<string> visited;

        q.push({start, 0});
        visited.insert(start);

        while (!q.empty()) {
            auto [current, steps] = q.front();
            q.pop();

            int zeroPos = current.find('0');

            for (int nextPos : moves[zeroPos]) {
                string nextState = current;
                swap(nextState[zeroPos], nextState[nextPos]);

                if (nextState == target) return steps + 1;

                if (visited.find(nextState) == visited.end()) {
                    q.push({nextState, steps + 1});
                    visited.insert(nextState);
                }
            }
        }

        return -1;
    }
};
```

## Approach 2: Modular BFS with Helper Functions

### Idea

This approach breaks the problem into modular steps for clarity and reusability:

1. **State Conversion**:

   - Convert the board into a string representation.

2. **Valid Moves Map**:

   - Use an unordered map to define the valid moves for each index.

3. **BFS Function**:
   - A standalone BFS function processes the board states and tracks levels (steps).

### Algorithm

1. Convert the 2D board to a string.
2. Create a map of valid moves for each position.
3. Use a BFS function to find the minimum moves:
   - Process each state level-wise.
   - Swap positions to generate new states.
   - Stop if the target is reached.

### Time Complexity

- **O(N! \* N)**:
  - N is the number of board cells (6 in this case).
  - Permutations of states multiplied by state generation.

### Space Complexity

- **O(N! + N)**: Space for the visited set, map, and queue.

### Code

```cpp
class Solution {
public:
    string boardToString(const vector<vector<int>>& board) {
        string result;
        for (const auto& row : board) {
            for (int num : row) {
                result += to_string(num);
            }
        }
        return result;
    }

    unordered_map<int, vector<int>> buildMoves() {
        return {
            {0, {1, 3}},
            {1, {0, 2, 4}},
            {2, {1, 5}},
            {3, {0, 4}},
            {4, {1, 3, 5}},
            {5, {2, 4}}
        };
    }

    int bfs(const string& start, const string& target, const unordered_map<int, vector<int>>& moves) {
        queue<string> que;
        unordered_set<string> visited;
        int level = 0;

        que.push(start);
        visited.insert(start);

        while (!que.empty()) {
            int n = que.size();

            // Process all states at the current level
            while (n--) {
                string curr = que.front();
                que.pop();

                if (curr == target) {
                    return level;
                }

                int indexOfZero = curr.find('0'); // Find the position of '0'
                for (int swapIdx : moves.at(indexOfZero)) {
                    string newState = curr;
                    swap(newState[indexOfZero], newState[swapIdx]);
                    if (visited.find(newState) == visited.end()) {
                        que.push(newState);
                        visited.insert(newState);
                    }
                }
            }
            level++;
        }

        return -1; // If no solution found
    }

    int slidingPuzzle(vector<vector<int>>& board) {
        string start = boardToString(board);
        string target = "123450";

        // Map of valid moves for each position
        unordered_map<int, vector<int>> moves = buildMoves();

        // Perform BFS to find the minimum moves
        return bfs(start, target, moves);
    }
};
```
