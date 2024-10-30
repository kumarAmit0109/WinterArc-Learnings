# 131. Combination Sum III

### Problem Link

[Combination Sum III - LeetCode](https://leetcode.com/problems/combination-sum-iii/description/)

## Approach: Recursive Backtracking

### Algorithm

1. **Recursive Backtracking**:
   - Utilize numbers from 1 to 9 to form combinations.
   - At each stage, make two calls: one including the current element and another excluding it.
2. **Base Cases**:
   - If the current sum equals `n` and the size of the current combination equals `k`, push the combination to the result.
   - If the current sum exceeds `n`, return immediately as no further calls are needed.
   - If the size of the current combination exceeds `k`, return without further processing.
3. **Iteration**:
   - Iterate through numbers from `start` to 9, attempting to include each in the current combination.

### Time Complexity

- The time complexity is approximately \(O(2^n)\) in the worst case, where `n` is the maximum depth of the recursion.

### Space Complexity

- The space complexity is \(O(k)\) due to the depth of the recursion stack and the space required to store valid combinations.

### Code

```cpp
void findCombinations(int start, int k, int n, int sum, vector<int>& currentCombination, vector<vector<int>>& result) {
    // Base case: if the sum equals n and the combination size is k
    if (sum == n && currentCombination.size() == k) {
        result.push_back(currentCombination);
        return;
    }

    // If the sum exceeds n or the combination size exceeds k, return
    if (sum > n || currentCombination.size() > k) {
        return;
    }

    // Loop through numbers 1 to 9 (as we only need single-digit numbers)
    for (int i = start; i <= 9; i++) {
        currentCombination.push_back(i);           // Include the current number
        findCombinations(i + 1, k, n, sum + i, currentCombination, result);  // Recur with the next number
        currentCombination.pop_back();              // Backtrack
    }
}

vector<vector<int>> combinationSum3(int k, int n) {
    vector<vector<int>> result;                 // To store the final combinations
    vector<int> currentCombination;              // To store the current combination
    findCombinations(1, k, n, 0, currentCombination, result); // Start with the first number
    return result;
}
```

# 132. Letter Combinations of a Phone Number

### Problem Link

[Letter Combinations of a Phone Number - LeetCode](https://leetcode.com/problems/letter-combinations-of-a-phone-number/description/)

## Approach: Recursive Backtracking

### Algorithm

1. **Mapping Digits to Letters**:
   - Use a mapping of phone digits to their corresponding letters (e.g., 2 → "abc", 3 → "def", etc.).
2. **Recursive Function**:
   - Create a recursive function `solve` that builds combinations based on the input digits.
3. **Base Case**:
   - If the index exceeds the length of the input digits, add the current output combination to the results.
4. **Digit Processing**:
   - Convert the character representation of each digit to an integer to access the corresponding letters from the mapping.
5. **Iteration**:
   - Loop through each letter mapped to the current digit, append it to the output, and make a recursive call for the next digit.
   - Backtrack by removing the last added letter after the recursive call.

### Time Complexity

- The time complexity is \(O(4^n)\), where \(n\) is the length of the input digits, as each digit can map to 3 or 4 letters.

### Space Complexity

- The space complexity is \(O(n)\), due to the recursive call stack and storing the combinations.

### Code

```cpp
// Recursive function to find combinations
void solve(string digits, string output, int index, vector<string>& ans, string mapping[]) {
    // Base case: if index exceeds the length of digits
    if (index >= digits.length()) {
        ans.push_back(output);
        return;
    }

    // Convert char to int for mapping
    int num = digits[index] - '0';
    string s = mapping[num]; // Get corresponding letters

    // Iterate through the letters
    for (int i = 0; i < s.length(); i++) {
        output.push_back(s[i]); // Include the current letter
        solve(digits, output, index + 1, ans, mapping); // Recur for the next digit
        output.pop_back(); // Backtrack
    }
}

vector<string> letterCombinations(string digits) {
    vector<string> ans; // To store the final combinations
    if (digits.length() == 0) {
        return ans; // Early return for empty input
    }
    string output = ""; // To store the current combination
    int index = 0;
    string mapping[10] = {"", "", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"};

    solve(digits, output, index, ans, mapping); // Start recursion
    return ans;
}
```

# 133. Palindrome Partitioning

### Problem Link

[Palindrome Partitioning - LeetCode](https://leetcode.com/problems/palindrome-partitioning/)

## Approach: Recursive Backtracking

### Algorithm

1. **Recursive Calls**:
   - The function `solve` generates all possible partitions of the string `s` starting from the index `start`.
   - For each index, it iterates through all possible end indices to form substrings.
2. **Palindrome Check**:

   - Use the helper function `isPalindrome` to check if a substring `s[start:end]` is a palindrome.
   - If the substring is a palindrome, add it to the `currentPartition`.

3. **Base Case**:

   - If the starting index exceeds the length of the string, the current partition is added to the result.

4. **Backtracking**:
   - After exploring all partitions that include the current substring, the last element is removed to backtrack.

### Time Complexity

- The time complexity is \(O(2^n)\) due to the exponential number of possible partitions.

### Space Complexity

- The space complexity is \(O(n)\) for the recursion stack, where \(n\) is the length of the string.

### Code

```cpp
void solve(int start, string& s, vector<string>& currentPartition, vector<vector<string>>& result) {
    // Base case: If we have reached the end of the string
    if (start >= s.size()) {
        result.push_back(currentPartition);
        return;
    }

    // Iterate through the string to generate substrings
    for (int end = start; end < s.size(); end++) {
        // Check if the substring s[start:end] is a palindrome
        if (isPalindrome(s, start, end)) {
            currentPartition.push_back(s.substr(start, end - start + 1)); // Add the palindrome to the current partition
            solve(end + 1, s, currentPartition, result); // Recur for the remaining substring
            currentPartition.pop_back(); // Backtrack
        }
    }
}

bool isPalindrome(const string& s, int left, int right) {
    while (left < right) {
        if (s[left] != s[right]) return false; // Not a palindrome
        left++;
        right--;
    }
    return true; // It's a palindrome
}

vector<vector<string>> partition(string s) {
    vector<vector<string>> result; // To store the final result
    vector<string> currentPartition; // To store the current partition
    solve(0, s, currentPartition, result); // Start backtracking from index 0
    return result; // Return the result
}
```

# 134. Word Search

### Problem Link

[Word Search - LeetCode](https://leetcode.com/problems/word-search/description/)

## Approach: Depth-First Search (DFS) with Backtracking

### Algorithm

1. **Recursive DFS Search**:
   - The helper function `solve` performs a DFS to check if the word exists in the board starting from a given cell `(i, j)`.
   - It verifies if each cell matches the current character in the word and then recursively explores the next character in four directions (up, down, left, right).
2. **Base Cases**:

   - If all characters in `word` have been matched (`k == word.length()`), return `true`.
   - If the current cell is out of bounds or does not match the current character of the word, return `false`.

3. **Backtracking**:

   - Mark the current cell as visited temporarily by setting it to a non-alphabetic character (e.g., `'\0'`).
   - After all recursive calls from the cell, restore its original character to allow other paths to use it.

4. **Iterating Over the Board**:
   - The `exist` function initiates the search from each cell of the board.
   - If a valid path for `word` is found starting from any cell, it returns `true`.

### Time Complexity

- \( O(M times N times 4^L) \), where \( M \) and \( N \) are the dimensions of the board and \( L \) is the length of the word.

### Space Complexity

- \( O(L) \), due to the recursive stack where \( L \) is the length of the word.

### Code

```cpp
bool solve(vector<vector<char>>& board, string& word, int i, int j, int k, int m, int n) {
    // Base case: If we've matched all characters in the word
    if (k == word.length()) {
        return true;
    }
    // Out of bounds or character mismatch
    if (i < 0 || i >= m || j < 0 || j >= n || board[i][j] != word[k]) {
        return false;
    }

    // Temporarily mark the cell as visited
    char temp = board[i][j];
    board[i][j] = '\0';

    // Explore all four directions
    bool found = solve(board, word, i + 1, j, k + 1, m, n) ||
                 solve(board, word, i - 1, j, k + 1, m, n) ||
                 solve(board, word, i, j + 1, k + 1, m, n) ||
                 solve(board, word, i, j - 1, k + 1, m, n);

    // Restore the cell's original value
    board[i][j] = temp;

    return found;
}

bool exist(vector<vector<char>>& board, string word) {
    int m = board.size();
    int n = board[0].size();

    // Try to find the word starting from each cell
    for (int i = 0; i < m; ++i) {
        for (int j = 0; j < n; ++j) {
            if (solve(board, word, i, j, 0, m, n)) {
                return true;
            }
        }
    }
    return false;
}
```

# 135. N-Queens

### Problem Link

[N-Queens - LeetCode](https://leetcode.com/problems/n-queens/description/)

## Approach 1: Recursive Backtracking with Safety Checks

### Algorithm

1. **Backtracking**: Place queens one column at a time. For each column, attempt to place a queen in every row.
2. **Safety Check**: Use the `isSafe` function to check three constraints:
   - No queen exists in the same row to the left.
   - No queen exists in the upper diagonal on the left.
   - No queen exists in the lower diagonal on the left.
3. **Recursive Call**: If a valid placement is found, place the queen and recurse for the next column.
4. **Backtrack**: If a solution is found or no valid placement is possible, backtrack by removing the queen and try the next position.

### Time Complexity

- \(O(N!)\): Since each queen has fewer possible placements as more queens are placed on the board.

### Space Complexity

- \(O(N^2)\): To store the board configuration.

### Code

```cpp
bool isSafe(int row, int col, vector<string>& board, int n) {
    // Check the same row on the left side
    for (int i = 0; i < col; i++) {
        if (board[row][i] == 'Q') return false;
    }

    // Check the upper diagonal on the left side
    for (int i = row, j = col; i >= 0 && j >= 0; i--, j--) {
        if (board[i][j] == 'Q') return false;
    }

    // Check the lower diagonal on the left side
    for (int i = row, j = col; i < n && j >= 0; i++, j--) {
        if (board[i][j] == 'Q') return false;
    }

    return true;
}

void solve(int col, vector<string>& board, vector<vector<string>>& solutions, int n) {
    if (col == n) {
        solutions.push_back(board);
        return;
    }

    for (int row = 0; row < n; row++) {
        if (isSafe(row, col, board, n)) {
            board[row][col] = 'Q';
            solve(col + 1, board, solutions, n);
            board[row][col] = '.';
        }
    }
}

vector<vector<string>> solveNQueens(int n) {
    vector<vector<string>> solutions;
    vector<string> board(n, string(n, '.'));
    solve(0, board, solutions, n);
    return solutions;
}
```

## Approach 2: Recursive Backtracking with Hashing for Optimized Safety Checks

### Algorithm

1. **Hash Arrays**: Utilize three hash arrays to mark rows and diagonals for queen placements:
   - `leftRow`: Tracks queens placed in rows.
   - `upperDiagonal`: Tracks queens in upper diagonals.
   - `lowerDiagonal`: Tracks queens in lower diagonals.
2. **Backtracking with Optimized Safety**:
   - For each column, try placing queens row by row.
   - Check the hash arrays to ensure the placement is safe.
3. **Recursive Call**: Place a queen and recursively attempt to place queens in the next column if safe.
4. **Backtrack**: Remove the queen and reset hash values when backtracking.

### Time Complexity

- \(O(N!)\): Similar to the basic backtracking approach but optimized with faster safety checks.

### Space Complexity

- \(O(N^2)\): To store board configurations, with an additional \(O(3N)\) for hash arrays.

### Code

```cpp
#include <vector>
#include <string>

using namespace std;

void solve(int col, vector<string>& board, vector<vector<string>>& solutions,
           vector<int>& leftRow, vector<int>& upperDiagonal, vector<int>& lowerDiagonal, int n) {
    if (col == n) {
        solutions.push_back(board);
        return;
    }

    for (int row = 0; row < n; row++) {
        if (leftRow[row] == 0 && lowerDiagonal[row + col] == 0 && upperDiagonal[n - 1 + col - row] == 0) {
            board[row][col] = 'Q';
            leftRow[row] = 1;
            lowerDiagonal[row + col] = 1;
            upperDiagonal[n - 1 + col - row] = 1;

            solve(col + 1, board, solutions, leftRow, upperDiagonal, lowerDiagonal, n);

            // Backtrack
            board[row][col] = '.';
            leftRow[row] = 0;
            lowerDiagonal[row + col] = 0;
            upperDiagonal[n - 1 + col - row] = 0;
        }
    }
}

vector<vector<string>> solveNQueens(int n) {
    vector<vector<string>> solutions;
    vector<string> board(n, string(n, '.'));

    vector<int> leftRow(n, 0);
    vector<int> upperDiagonal(2 * n - 1, 0);
    vector<int> lowerDiagonal(2 * n - 1, 0);

    solve(0, board, solutions, leftRow, upperDiagonal, lowerDiagonal, n);
    return solutions;
}
```
