# 66. Row with Maximum 1's

### Problem Link

[Row with Maximum 1's - Naukri](https://www.naukri.com/code360/problems/row-with-maximum-1-s_1112656?)

## Approach 1: Brute Force

### Idea

1. **Declare variables:**

   - `cnt_max` (initialized to 0) to store the maximum count of 1’s found.
   - `index` (initialized to -1) to store the row number with the most 1’s.

2. **Iterate through each row:**

   - For every row, count the number of 1’s using a nested loop.

3. **Update the maximum count:**

   - If the count of 1’s in the current row is greater than `cnt_max`, update both `cnt_max` and `index`.

4. **Return `index`:**
   - If no 1's are found, the result remains -1. Otherwise, return the index of the row with the most 1's.

### Time Complexity

- **O(n \* m)** where `n` is the number of rows and `m` is the number of columns.

### Space Complexity

- **O(1)** as no extra space is used.

### Code

```cpp
int rowWithMax1s(vector<vector<int>> &matrix, int n, int m) {
    int cnt_max = 0, index = -1;

    for (int i = 0; i < n; i++) {
        int count = 0;
        for (int j = 0; j < m; j++) {
            if (matrix[i][j] == 1) {
                count++;
            }
        }

        if (count > cnt_max) {
            cnt_max = count;
            index = i;
        }
    }

    return index;
}
```

## Approach 2: Using Binary Search (lowerBound)

### Idea

1. **Declare variables:**

   - `cnt_max` (initialized to 0) to store the maximum count of 1’s found.
   - `index` (initialized to -1) to store the row number with the most 1’s.

2. **Use Binary Search (lowerBound) for each row:**

   - For every row, find the first occurrence of 1 using `lower_bound()`.

3. **Calculate the number of 1’s:**

   - Use the formula:  
     `Number_of_ones = m - lowerBound(1)` (where `m` is the number of columns).

4. **Update the maximum count:**

   - If the count of 1’s in the current row is greater than `cnt_max`, update both `cnt_max` and `index`.

5. **Return `index`:**
   - If no 1's are found, the result remains -1. Otherwise, return the index of the row with the most 1's.

### Time Complexity

- **O(n \* log(m))** where `n` is the number of rows and `m` is the number of columns.

### Space Complexity

- **O(1)** as no extra space is used.

### Code

```cpp
int rowWithMax1s(vector<vector<int>> &matrix, int n, int m) {
    int cnt_max = 0, index = -1;

    for (int i = 0; i < n; i++) {
        auto it = lower_bound(matrix[i].begin(), matrix[i].end(), 1);
        int ones_count = m - (it - matrix[i].begin());

        if (ones_count > cnt_max) {
            cnt_max = ones_count;
            index = i;
        }
    }

    return index;
}
```

# 67. Search a 2D Matrix

### Problem Link

[Search a 2D Matrix - LeetCode](https://leetcode.com/problems/search-a-2d-matrix/description/)

## Approach 1: Brute Force

### Idea

1. **Iterate through each row:**

   - Use a loop (say `i`) to select a particular row at a time.

2. **Check each element:**

   - For every row, use another loop (say `j`) to traverse each column.
   - Inside the loops, check if `matrix[i][j]` is equal to the `target`.
   - If a matching element is found, return `true`.

3. **Return `false`:**

   - If the target is not found after traversing the entire matrix, return `false`.

### Time Complexity

- **O(n \* m)** where `n` is the number of rows and `m` is the number of columns.

### Space Complexity

- **O(1)** as no extra space is used.

### Code

```cpp
bool searchMatrix(vector<vector<int>>& matrix, int target) {
    for (int i = 0; i < matrix.size(); i++) {
        for (int j = 0; j < matrix[0].size(); j++) {
            if (matrix[i][j] == target) {
                return true;
            }
        }
    }
    return false;
}
```

## Approach 2: Binary Search on Each Row

### Idea

1. **Define a binary search function:**

   - Create a function `binarySearch` that takes a row and the `target` as parameters.

2. **Check boundaries:**

   - For each row in the matrix, check if the `target` is within the range defined by the first and last elements of the row.

3. **Perform binary search:**

   - If the target is within the boundaries, call the `binarySearch` function on that row.

4. **Return `true`:**

   - If the target is found in any row, return `true`.

5. **Return `false`:**

   - If the target is not found after searching all rows, return `false`.

### Time Complexity

- **O(n \* log(m))** where `n` is the number of rows and `m` is the number of columns.

### Space Complexity

- **O(1)** as no extra space is used.

### Code

```cpp
bool binarySearch(const vector<int>& row, int target) {
    int left = 0, right = row.size() - 1;
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (row[mid] == target) {
            return true;
        } else if (row[mid] < target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    return false;
}

bool searchMatrix(vector<vector<int>>& matrix, int target) {
    int n = matrix[0].size();
    for (const auto& row : matrix) {
        if (row[0] <= target && row[n - 1] >= target) {
            return binarySearch(row, target);
        }
    }
    return false;
}
```

## Approach 3: Binary Search on the Entire Matrix

### Idea

1. **Calculate the number of rows and columns:**

   - Get the number of rows and columns in the matrix.

2. **Set up binary search:**

   - Initialize `start` to 0 and `end` to `row * col - 1`.

3. **Perform binary search:**

   - While `start` is less than or equal to `end`:
     - Calculate the mid index.
     - Convert the mid index into row and column indices.
     - Compare the element at the mid index with the `target`.

4. **Return `true`:**

   - If the target is found, return `true`.

5. **Update the search space:**

   - If the mid element is less than the target, move `start` to `mid + 1`.
   - If the mid element is greater than the target, move `end` to `mid - 1`.

6. **Return `false`:**

   - If the target is not found after the search completes, return `false`.

### Time Complexity

- **O(log(n \* m))** where `n` is the number of rows and `m` is the number of columns.

### Space Complexity

- **O(1)** as no extra space is used.

### Code

```cpp
bool searchMatrix(vector<vector<int>>& matrix, int target) {
    int row = matrix.size();
    int col = matrix[0].size();
    int start = 0, end = row * col - 1;

    while (start <= end) {
        int mid = start + (end - start) / 2;
        int element = matrix[mid / col][mid % col];

        if (element == target) {
            return true;
        } else if (element < target) {
            start = mid + 1;
        } else {
            end = mid - 1;
        }
    }

    return false;
}
```

# 68. Search a 2D Matrix II

### Problem Link

[Search a 2D Matrix II - LeetCode](https://leetcode.com/problems/search-a-2d-matrix-ii/description/)

## Approach 1: Brute Force

### Idea

1. **Iterate through each row:**

   - Use a loop (say `i`) to select a particular row at a time.

2. **Check each element:**

   - For every row, use another loop (say `j`) to traverse each column.

3. **Match target:**

   - Inside the loops, check if the element `matrix[i][j]` is equal to the `target`. If a match is found, return `true`.

4. **Return false:**

   - If no matches are found after completing the traversal of all rows, return `false`.

### Time Complexity

- **O(n \* m)** where `n` is the number of rows and `m` is the number of columns.

### Space Complexity

- **O(1)** as no extra space is used.

### Code

```cpp
bool searchMatrix(vector<vector<int>>& matrix, int target) {
    int n = matrix.size();
    int m = matrix[0].size();

    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            if (matrix[i][j] == target) {
                return true;
            }
        }
    }
    return false;
}
```

## Approach 2: Binary Search on Each Row

### Idea

1. **Iterate through each row:**

   - Use a loop (say `i`) to select a particular row at a time.

2. **Check using binary search:**

   - For every row `i`, check if it contains the `target` using binary search.

3. **Match target:**

   - After applying binary search on row `i`, if an element equal to the `target` is found, return `true`. Otherwise, move on to the next row.

4. **Return false:**

   - If no matches are found after checking all rows, return `false`.

### Time Complexity

- **O(n \* log(m))** where `n` is the number of rows and `m` is the number of columns.

### Space Complexity

- **O(1)** as no extra space is used.

### Code

```cpp
bool binarySearch(const vector<int>& row, int target) {
    int left = 0, right = row.size() - 1;
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (row[mid] == target) {
            return true;
        } else if (row[mid] < target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    return false;
}

bool searchMatrix(vector<vector<int>>& matrix, int target) {
    for (const auto& row : matrix) {
        if (binarySearch(row, target)) {
            return true;
        }
    }
    return false;
}
```

## Approach 3: Search in a Zigzag Manner

### Idea

1. **Define boundaries:**

   - Initialize `startrow`, `endrow`, `startcol`, and `endcol` to represent the boundaries of the matrix.

2. **Iterate through the matrix:**

   - While the `startcol` is less than or equal to `endcol` and `startrow` is less than or equal to `endrow`, perform the following steps:

3. **Check middle element:**

   - Get the middle element `mid = matrix[startrow][endcol]`.

4. **Compare with target:**

   - If `mid` is equal to `target`, return `true`.
   - If `mid` is greater than `target`, decrement `endcol`.
   - If `mid` is less than `target`, increment `startrow`.

5. **Return false:**

   - If the loop completes without finding the target, return `false`.

### Time Complexity

- **O(n + m)** where `n` is the number of rows and `m` is the number of columns.

### Space Complexity

- **O(1)** as no extra space is used.

### Code

```cpp
bool searchMatrix(vector<vector<int>>& matrix, int target) {
    int row = matrix.size();
    int col = matrix[0].size();

    int startrow = 0;
    int endrow = row - 1;
    int startcol = 0;
    int endcol = col - 1;

    while (startcol <= endcol && startrow <= endrow) {
        int mid = matrix[startrow][endcol];
        if (mid == target) {
            return true;
        }
        if (mid > target) {
            endcol--;
        } else {
            startrow++;
        }
    }
    return false;
}
```
