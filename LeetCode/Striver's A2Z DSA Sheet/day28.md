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

# 137. Word Break

### Problem Link

[Word Break - LeetCode](https://leetcode.com/problems/word-break/submissions/1438874176/)

## Approach 1: Recursive Backtracking

### Algorithm

1. Start from the beginning of the string `s`.
2. For each possible substring from `start` to `end`, check if it is in the dictionary.
3. If a valid word is found, recursively call the helper function from `end`.
4. If `start` reaches the end of the string, return `true`.
5. If no valid segmentation is found, return `false`.

### Time Complexity

- \(O(2^n)\): Exponential due to overlapping subproblems without memoization.

### Space Complexity

- \(O(n)\): Stack space due to recursion.

### Code

```cpp
bool wordBreakHelper(string& s, unordered_set<string>& dict, int start) {
    if (start == s.length()) return true;

    for (int end = start + 1; end <= s.length(); ++end) {
        string word = s.substr(start, end - start);
        if (dict.count(word) && wordBreakHelper(s, dict, end)) {
            return true;
        }
    }

    return false;
}

bool wordBreak(string s, vector<string>& wordDict) {
    unordered_set<string> dict(wordDict.begin(), wordDict.end());
    return wordBreakHelper(s, dict, 0);
}
```

## Approach 2: Recursive Backtracking with Memoization

### Algorithm

1. Start from the beginning of the string `s`.
2. For each possible substring from `start` to `end`, check if it is in the dictionary.
3. Use a memoization array `memo` to store the result for each starting index to avoid redundant calculations.
4. If a valid word is found, recursively call the helper function from `end`.
5. If `start` reaches the end of the string, return `true`.
6. If no valid segmentation is found, mark `memo[start]` as `false` and return.

### Time Complexity

- \(O(n^2)\): Each substring checked once, with memoization optimizing repeated checks.

### Space Complexity

- \(O(n)\): For memoization and recursion stack.

### Code

```cpp
bool wordBreakHelper(string& s, unordered_set<string>& dict, int start, vector<int>& memo) {
    if (start == s.length()) return true;
    if (memo[start] != -1) return memo[start];

    for (int end = start + 1; end <= s.length(); ++end) {
        string word = s.substr(start, end - start);
        if (dict.count(word) && wordBreakHelper(s, dict, end, memo)) {
            return memo[start] = true;
        }
    }

    return memo[start] = false;
}

bool wordBreak(string s, vector<string>& wordDict) {
    unordered_set<string> dict(wordDict.begin(), wordDict.end());
    vector<int> memo(s.length(), -1);
    return wordBreakHelper(s, dict, 0, memo);
}
```

## Approach 3: Dynamic Programming

### Algorithm

1. Initialize a DP array `dp` of size `n + 1`, where `n` is the length of `s`. Set `dp[0]` to `true` as the base case (an empty string can be segmented).
2. Iterate through each index `i` from 1 to `n`. For each `i`, iterate through all previous positions `j` from 0 to `i`.
3. For each `j`, if `dp[j]` is `true` (meaning `s[0:j]` can be segmented) and `s[j:i]` exists in the dictionary, set `dp[i]` to `true`.
4. Continue this until the end of the string.
5. Return `dp[n]`, which indicates if the entire string `s` can be segmented using words from the dictionary.

### Time Complexity

- \(O(n^2)\): We evaluate every possible substring from `j` to `i`.

### Space Complexity

- \(O(n)\): For the DP array.

### Code

```cpp
bool wordBreak(string s, vector<string>& wordDict) {
    unordered_set<string> dict(wordDict.begin(), wordDict.end());
    int n = s.length();
    vector<bool> dp(n + 1, false);
    dp[0] = true; // base case: an empty string can be segmented

    for (int i = 1; i <= n; ++i) {
        for (int j = 0; j < i; ++j) {
            if (dp[j] && dict.count(s.substr(j, i - j))) {
                dp[i] = true;
                break;
            }
        }
    }

    return dp[n];
}
```

# 138. M Coloring Problem

### Problem Link

[M Coloring Problem - Naukri Code360](https://www.naukri.com/code360/problems/m-coloring-problem_981273)

## Approach: Recursive Backtracking with Validity Check

### Algorithm

1. **Recursive Coloring**:
   - Start at each node and attempt to assign colors from `1` to `m`.
   - For each color, check if it’s valid by ensuring no adjacent nodes have the same color.
   - If a valid color is assigned, proceed recursively to the next node.
2. **Base Case**:
   - If all nodes are successfully colored, return "YES".
   - If unable to color with the given constraints, backtrack and try a different color.
3. **Validity Check**:
   - Ensure that no adjacent node has the same color before assigning it to the current node.

### Time Complexity

- \( O(m^N) \), where \( N \) is the number of nodes, due to the recursive backtracking for each color assignment.

### Space Complexity

- \( O(N) \), for the recursion stack depth.

### Code

```cpp
#include <vector>
#include <string>
bool isSafe(int node, vector<vector<int>> &mat, vector<int> &colors, int color) {
    for (int i = 0; i < mat.size(); i++) {
        if (mat[node][i] == 1 && colors[i] == color) {  // Adjacent node has the same color
            return false;
        }
    }
    return true;
}

bool solve(int node, vector<vector<int>> &mat, int m, vector<int> &colors) {
    if (node == mat.size()) {
        return true;  // All nodes colored successfully
    }

    for (int color = 1; color <= m; color++) {
        if (isSafe(node, mat, colors, color)) {
            colors[node] = color;
            if (solve(node + 1, mat, m, colors)) {
                return true;
            }
            colors[node] = 0;  // Backtrack
        }
    }
    return false;
}

string graphColoring(vector<vector<int>> &mat, int m) {
    int n = mat.size();
    vector<int> colors(n, 0);
    if (solve(0, mat, m, colors)) {
        return "YES";
    }
    return "NO";
}
```

# 139. Sudoku Solver

### Problem Link

[Sudoku Solver - LeetCode](https://leetcode.com/problems/sudoku-solver/description/)

## Approach: Recursive Backtracking

### Algorithm

1. **Recursive Backtracking**:
   - Traverse each cell of the board.
   - When an empty cell is found:
     - Try placing each number (1-9).
     - For each number, check if placing it in the current cell keeps the board valid.
2. **Helper Function (`isValid`)**:
   - Checks the row, column, and 3x3 subgrid to ensure no duplicates of the current number.
3. **Base Case**:
   - If all cells are filled without conflict, the board is solved.
4. **Backtracking**:
   - If placing a number does not lead to a solution, reset the cell to empty and try the next number.

### Time Complexity

The worst-case time complexity for solving Sudoku is \(O(9^{N})\), where \(N\) is the number of empty cells on the board (up to 81). This arises because, in the worst case, you may have to try every possible digit (1-9) for every empty cell. However, since the board is limited to a maximum of 81 cells, the complexity can be expressed as:

- **TC = O(9^{M})**, where \(M\) is the number of empty cells (which can be at most 81).

In practice, due to the constraints of Sudoku, the average-case performance is much better because many branches of the recursion will be pruned by the validity checks.

### Space Complexity

The space complexity is mainly determined by the recursion stack due to the backtracking approach. The maximum depth of the recursion stack is equal to the number of empty cells:

- **SC = O(N)**, where \(N\) is the number of empty cells (up to 81).

Additionally, the input board itself occupies \(O(1)\) space since its size is fixed (9x9). Therefore, the space complexity is primarily driven by the recursion depth.

### Code

```cpp
#include <vector>
using namespace std;

bool isValid(vector<vector<char>>& board, int row, int col, char num) {
    for (int i = 0; i < 9; i++) {
        // Check the row and column for duplicates
        if (board[row][i] == num || board[i][col] == num) {
            return false;
        }

        // Check the 3x3 subgrid for duplicates
        int subRow = 3 * (row / 3) + i / 3;
        int subCol = 3 * (col / 3) + i % 3;
        if (board[subRow][subCol] == num) {
            return false;
        }
    }
    return true;
}

bool solve(vector<vector<char>>& board) {
    for (int row = 0; row < 9; row++) {
        for (int col = 0; col < 9; col++) {
            // Find the next empty cell
            if (board[row][col] == '.') {
                // Try all possible values from '1' to '9'
                for (char num = '1'; num <= '9'; num++) {
                    if (isValid(board, row, col, num)) {
                        board[row][col] = num; // Place the number

                        // Recursively try to solve the rest of the board
                        if (solve(board)) {
                            return true; // If successful, return true
                        }

                        // Backtrack: remove the number
                        board[row][col] = '.';
                    }
                }
                return false; // No valid number found for this cell
            }
        }
    }
    return true; // All cells are filled successfully
}

void solveSudoku(vector<vector<char>>& board) {
    solve(board);
}
```

# 140. Expression Add Operators

### Problem Link

[Expression Add Operators - LeetCode](https://leetcode.com/problems/expression-add-operators/description/)

## Approach: Backtracking

### Algorithm

1. **Recursive Function** (`solve`):

   - Traverse the string `num` to generate possible expressions.
   - At each index, consider every possible substring as a number.
   - Use operators (`+`, `-`, `*`) to combine numbers into an expression.
   - Keep track of the current value of the expression and the last value used for multiplication.

2. **Base Case**:

   - If the end of the string is reached (`index == num.size()`), check if the current value equals the target. If so, add the expression to the results.

3. **Avoid Leading Zeros**:

   - Skip substrings that represent numbers with leading zeros (except for the number "0" itself).

4. **Backtracking**:
   - After exploring each operator, revert the current expression to its previous state to explore other possibilities.

### Time Complexity

- \(O(4^n)\): Each number can be followed by one of three operators or no operator at all, leading to exponential combinations.

### Space Complexity

- \(O(n)\): The space used for the current expression and the recursive call stack.

### Code

```cpp
void solve(const string& num, int target, int index, long long currentValue, long long lastValue, string& currentExpression, vector<string>& ans) {
    if (index == num.size()) {
        if (currentValue == target) {
            ans.push_back(currentExpression);
        }
        return;
    }

    for (int i = index; i < num.size(); ++i) {
        // To avoid leading zeros
        if (i > index && num[index] == '0') break;

        string currentNum = num.substr(index, i - index + 1);
        long long currentNumber = stoll(currentNum);

        if (index == 0) {
            // First number, we cannot add an operator before it
            currentExpression = currentNum;
            solve(num, target, i + 1, currentNumber, currentNumber, currentExpression, ans);
        } else {
            // Try addition
            currentExpression += '+' + currentNum;
            solve(num, target, i + 1, currentValue + currentNumber, currentNumber, currentExpression, ans);
            currentExpression.resize(currentExpression.size() - currentNum.size() - 1); // backtrack

            // Try subtraction
            currentExpression += '-' + currentNum;
            solve(num, target, i + 1, currentValue - currentNumber, -currentNumber, currentExpression, ans);
            currentExpression.resize(currentExpression.size() - currentNum.size() - 1); // backtrack

            // Try multiplication
            currentExpression += '*' + currentNum;
            solve(num, target, i + 1, currentValue - lastValue + (lastValue * currentNumber), lastValue * currentNumber, currentExpression, ans);
            currentExpression.resize(currentExpression.size() - currentNum.size() - 1); // backtrack
        }
    }
}

vector<string> addOperators(string num, int target) {
    vector<string> ans;
    string currentExpression;
    solve(num, target, 0, 0, 0, currentExpression, ans);
    return ans;
}
```
