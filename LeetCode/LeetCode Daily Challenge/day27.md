# Count Square Submatrices with All Ones

**Problem Link**: [Count Square Submatrices with All Ones](https://leetcode.com/problems/count-square-submatrices-with-all-ones)

## Approach

### Approach 1: Recursive Solution without Memoization

1. **Base Case**:
   - If the current cell is out of bounds or contains a 0, return 0.
2. **Recursive Exploration**:
   - For a cell containing 1, recursively explore:
     - The right cell.
     - The bottom cell.
     - The diagonal cell.
   - The number of square submatrices ending at the current cell is `1 + min(right, below, diagonal)`.
3. **Count Squares**:
   - For each cell in the matrix, if it contains 1, add the result of the `solve` function to the total count.

### Algorithm Steps

1. Initialize the dimensions of the matrix.
2. Iterate through each cell in the matrix.
3. For each cell with value 1, call the recursive `solve` function.
4. Sum the results to get the total count of square submatrices.

### Time Complexity

1. **Outer Loop**:

   - The `countSquares` function iterates through each cell of the \(m \times n\) matrix using two nested loops, which contributes \(O(m \times n)\) to the time complexity.

2. **Recursive Function Calls**:

   - The `solve` function is called recursively for each cell containing a `1`. Each call explores three directions: right, diagonal, and below.
   - In the worst case, this can lead to a time complexity of \(O(3^{mn})\) due to overlapping subproblems.
   - However, considering practical limits, the complexity can be approximated to \(O(m \times n \times \text{max}(m, n))\) depending on the average depth of recursion.

3. **Overall Time Complexity**:
   - **Approximation**: \(O(m \times n \times \text{max}(m, n))\)

### Space Complexity

1. **Recursion Stack**:

   - The maximum depth of recursion can reach \(O(m + n)\) as the function can move diagonally down to the last row and column.

2. **No Extra Data Structures**:

   - The implementation does not utilize any additional data structures beyond the input matrix.

3. **Overall Space Complexity**:
   - **Approximation**: \(O(m + n)\)

### Summary

- **Time Complexity**: \(O(m \times n \times \text{max}(m, n))\)
- **Space Complexity**: \(O(m + n)\)

### Code Implementation

```cpp
class Solution {
private:
    int m, n;

    int solve(int i, int j, vector<vector<int>>& matrix) {
        if (i >= m || j >= n) {
            return 0;
        }
        if (matrix[i][j] == 0) {
            return 0;
        }
        int right = solve(i, j + 1, matrix);
        int diag = solve(i + 1, j + 1, matrix);
        int below = solve(i + 1, j, matrix);
        return 1 + min({right, diag, below});
    }

public:
    int countSquares(vector<vector<int>>& matrix) {
        m = matrix.size();
        n = matrix[0].size();
        int result = 0;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (matrix[i][j] == 1) {
                    result += solve(i, j, matrix);
                }
            }
        }
        return result;
    }
};
```

### Approach 2: Recursive Solution with Memoization

1. **Memoization**:
   - Use a memoization table to store results of subproblems, reducing redundant calculations.
2. **Recursive Exploration**:
   - Similar to Approach 1, but check the memoization table before computing the result for a cell.
3. **Count Squares**:
   - For each cell with value 1, add the result from the `solve` function to the total count, using memoization for efficiency.

### Algorithm Steps

1. Initialize the dimensions of the matrix and the memoization table.
2. Iterate through each cell in the matrix.
3. For each cell with value 1, call the recursive `solve` function with memoization.
4. Sum the results to get the total count of square submatrices.

### Time Complexity

- **O(m \* n)**: where \( m \) is the number of rows and \( n \) is the number of columns in the matrix. Each cell is computed only once due to memoization.

### Space Complexity

- **O(m \* n)**: The space used for the memoization table to store results for each cell.

### Code Implementation

```cpp
class Solution {
private:
    int m, n;
    vector<vector<int>> memo;

    int solve(int i, int j, vector<vector<int>>& matrix) {
        if (i >= m || j >= n) {
            return 0;
        }
        if (matrix[i][j] == 0) {
            return 0;
        }
        if (memo[i][j] != -1) {
            return memo[i][j];
        }
        int right = solve(i, j + 1, matrix);
        int diag = solve(i + 1, j + 1, matrix);
        int below = solve(i + 1, j, matrix);
        return memo[i][j] = 1 + min({right, diag, below});
    }

public:
    int countSquares(vector<vector<int>>& matrix) {
        m = matrix.size();
        n = matrix[0].size();
        memo = vector<vector<int>>(m, vector<int>(n, -1));
        int result = 0;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (matrix[i][j] == 1) {
                    result += solve(i, j, matrix);
                }
            }
        }
        return result;
    }
};
```

### Approach 3: Dynamic Programming

1. **Dynamic Programming Table**:
   - Use a 2D DP table to keep track of the size of the largest square submatrix ending at each cell.
2. **Square Counting**:
   - If the current cell contains a 1, calculate the size of the square submatrix that can be formed by considering the top, left, and top-left neighboring cells.

### Algorithm Steps

1. Initialize the dimensions of the matrix and create a DP table filled with zeros.
2. Iterate through each cell in the matrix.
3. If a cell contains a 1:
   - If it’s in the first row or first column, it can only form a 1x1 square.
   - Otherwise, set the DP value to 1 plus the minimum of the top, left, and top-left values.
4. Accumulate the result by adding the values in the DP table.

### Time Complexity

- **O(m \* n)**: where \( m \) is the number of rows and \( n \) is the number of columns in the matrix. Each cell is processed once.

### Space Complexity

- **O(m \* n)**: The space used for the DP table to store results for each cell.

### Code Implementation

```cpp
class Solution {
public:
    int countSquares(vector<vector<int>>& matrix) {
        int m = matrix.size(), n = matrix[0].size();
        vector<vector<int>> dp(m, vector<int>(n, 0));
        int result = 0;

        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (matrix[i][j] == 1) {
                    if (i == 0 || j == 0) {
                        dp[i][j] = 1;  // Edge cells can only form 1x1 squares
                    } else {
                        dp[i][j] = 1 + min({dp[i - 1][j], dp[i][j - 1], dp[i - 1][j - 1]});
                    }
                    result += dp[i][j];
                }
            }
        }
        return result;
    }
};
```
