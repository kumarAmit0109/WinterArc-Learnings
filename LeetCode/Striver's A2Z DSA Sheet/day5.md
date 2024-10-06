# 21. Next Permutation

## Problem Link

[LeetCode - Next Permutation](https://leetcode.com/problems/next-permutation/description/)

## Approach 1: Generate All Permutations (Brute Force)

### Idea

In this approach, we generate all unique permutations of the given array and then determine the next permutation based on the current arrangement.

### Steps:

1. **Function to Generate All Permutations**:

   - Use a recursive function to generate all unique permutations of the array `nums`.
   - Store the permutations in a set to ensure uniqueness.

2. **Find the Current Permutation**:

   - Locate the current permutation in the set of generated permutations.

3. **Return the Next Permutation**:
   - If the current permutation is not the last one, return the next one.
   - If it is the last permutation, return the first one.

### Code Implementation:

```cpp
// Function to generate all permutations
void generatePermutations(vector<int>& nums, int start, set<vector<int>>& result) {
    if (start == nums.size() - 1) {
        result.insert(nums);
        return;
    }
    for (int i = start; i < nums.size(); i++) {
        swap(nums[start], nums[i]);  // Swap to create a new permutation
        generatePermutations(nums, start + 1, result); // Recursive call for the next index
        swap(nums[start], nums[i]);  // Backtrack to the original array
    }
}

void nextPermutation(vector<int>& nums) {
    // Step 1: Generate all unique permutations
    set<vector<int>> permutations;
    generatePermutations(nums, 0, permutations);

    // Step 2: Find the current permutation
    auto it = permutations.find(nums);

    // Step 3: If it's not the last permutation, return the next one
    if (it != permutations.end() && next(it) != permutations.end()) {
        nums = *next(it);
    } else {
        // If it's the last permutation, return the first one
        nums = *permutations.begin();
    }
}
```

## Approach 2: Using Built-in Function

### Idea

C++ provides an in-built function called `next_permutation()` that directly returns the lexicographically next greater permutation of the input array. This approach is efficient and straightforward, eliminating the need to manually generate all permutations.

### Steps:

**Utilize the `next_permutation()` Function**:

- Call the `next_permutation()` function on the input vector to rearrange it into the next permutation.

### Code Implementation:

```cpp
void nextPermutation(std::vector<int>& nums) {
    std::next_permutation(nums.begin(), nums.end());
}
```

## Approach 3: Manual Algorithm

### Idea

This approach involves a systematic way to find the next lexicographical permutation without generating all permutations. The algorithm follows these steps:

### Steps:

1. **Find the Pivot**:

   - Traverse the array from right to left to find the first index `i` such that `nums[i] < nums[i + 1]`. This index `i` is called the pivot.

2. **Find the Successor**:

   - If a pivot is found, traverse the array from right to left again to find the first index `j` such that `nums[j] > nums[i]`. This element will be the successor to swap with the pivot.

3. **Swap**:

   - Swap the elements at indices `i` and `j`.

4. **Reverse the Suffix**:

   - Reverse the subarray from `i + 1` to the end of the array to ensure the smallest lexicographical order.

5. **Edge Case**:
   - If no pivot is found (meaning the array is in descending order), simply reverse the entire array to get the smallest permutation.

### Why This Approach Works:

This approach works because:

- Finding the pivot allows us to identify the first part of the permutation that can be altered to find the next permutation.
- Swapping the pivot with its successor ensures we get the smallest possible number greater than the current permutation.
- Reversing the suffix guarantees we form the next smallest permutation since the suffix will be in descending order and will be rearranged to the smallest possible order.

### Code Implementation:

```cpp
void nextPermutation(vector<int>& nums) {
    int n = nums.size();
    int i = n - 2;

    // Find the first pair from the right where nums[i] < nums[i+1]
    while (i >= 0 && nums[i] >= nums[i + 1]) {
        i--;
    }

    if (i >= 0) {
        int j = n - 1;
        // Find the smallest number on the right side of nums[i] that is greater than nums[i]
        while (j >= 0 && nums[j] <= nums[i]) {
            j--;
        }
        // Swap nums[i] and nums[j]
        swap(nums[i], nums[j]);
    }

    // Reverse the subarray to the right of i
    reverse(nums.begin() + i + 1, nums.end());
}

```

# 22. Leaders in an Array

## Problem Link

[GeeksforGeeks - Problem Link](https://www.geeksforgeeks.org/problems/leaders-in-an-array-1587115620/1)

## Approach 1: Brute Force

### Idea

In this approach, we use two nested loops to check whether an element is a leader by comparing it with all the subsequent elements in the array.

### Time Complexity

- **Time Complexity:** O(n²)  
  We use two loops, leading to a quadratic time complexity.

### Space Complexity

- **Space Complexity:** O(1)  
  We use only a few extra variables.

### Code

```cpp
vector<int> leaders(int n, int arr[]) {
    vector<int> ans;

    for (int i = 0; i < n; i++) {
        bool isLeader = true;

        for (int j = i + 1; j < n; j++) {
            if (arr[i] < arr[j]) {
                isLeader = false;
                break; // No need to check further
            }
        }

        if (isLeader) {
            ans.push_back(arr[i]);
        }
    }

    return ans;
}
```

## Approach 2: Optimized Approach

### Idea

This approach traverses the array from the end, keeping track of the maximum element seen so far. If the current element is greater than or equal to this maximum, it is considered a leader.

### Time Complexity

- **Time Complexity:** O(n)  
  We traverse the array once.

### Space Complexity

- **Space Complexity:** O(n)  
  We use a vector to store the leaders.

### Code

```cpp
vector<int> leaders(int n, int arr[]) {
    vector<int> ans;
    int maxi = INT_MIN;

    // Traverse the array from the end
    for (int i = n - 1; i >= 0; i--) {
        // Check if the current element is greater than the current maximum
        if (arr[i] >= maxi) {
            maxi = arr[i]; // Update maximum
            ans.push_back(maxi); // Add to leaders
        }
    }

    // Reverse the result to maintain the order of leaders
    reverse(ans.begin(), ans.end());

    return ans;
}
```

# 23. Longest Consecutive Sequence

## Problem Link

[LeetCode - Problem Link](https://leetcode.com/problems/longest-consecutive-sequence/description/)

## Approach 1: Brute Force with Linear Search

### Idea

We iterate through each element and for every element `x`, we search for consecutive elements like `x+1`, `x+2`, `x+3`, and so on in the array using a linear search. We increment the sequence length (`cnt`) as we find consecutive elements.

### Time Complexity

- **Time Complexity:** O(n²)  
  For each element, we search the remaining array for consecutive numbers.

### Space Complexity

- **Space Complexity:** O(1)  
  We use only a few extra variables for counting.

### Code

```cpp
int longestConsecutive(vector<int>& nums) {
    int maxLength = 0;

    for (int i = 0; i < nums.size(); i++) {
        int x = nums[i];
        int count = 1;

        while (find(nums.begin(), nums.end(), x + 1) != nums.end()) {
            count++;
            x++;
        }

        maxLength = max(maxLength, count);
    }

    return maxLength;
}
```

## Approach 2: Sorting-Based Approach

### Idea

We sort the array first, then iterate through it to count the length of consecutive sequences. If two elements are consecutive (`nums[i] - nums[i - 1] == 1`), we increment the count. If they are the same, we skip duplicates.

### Time Complexity

- **Time Complexity:** O(n log n)  
  Sorting the array takes `O(n log n)` time.

### Space Complexity

- **Space Complexity:** O(1)  
  We use only a few extra variables.

### Code

```cpp
int longestConsecutive(vector<int>& nums) {
    if (nums.empty()) return 0;  // Handle empty input

    // Sort the array
    sort(nums.begin(), nums.end());

    int count = 1;  // Start with 1 for the first element
    int maxCount = 1;  // Initialize maxCount

    for (int i = 1; i < nums.size(); i++) {
        if (nums[i] == nums[i - 1]) {
            continue;  // Skip duplicates
        } else if (nums[i] - nums[i - 1] == 1) {
            count++;  // Increase count for consecutive numbers
        } else {
            maxCount = max(maxCount, count);  // Update maxCount
            count = 1;  // Reset count for the new sequence
        }
    }
    maxCount = max(maxCount, count);  // Check the last sequence

    return maxCount;
}
```

## Approach 3: Set-Based Approach

### Idea

We use a set to store the elements of the array. For each element, we check if it is the start of a sequence by verifying if the previous element (`num - 1`) does not exist in the set. If it is the start of a sequence, we count the length of consecutive numbers.

### Time Complexity

- **Time Complexity:** O(n)  
  Each element is processed once, and searching in a set takes O(1) on average.

### Space Complexity

- **Space Complexity:** O(n)  
  A set is used to store the elements.

### Code

```cpp
int longestConsecutive(vector<int>& nums) {
    unordered_set<int> numSet(nums.begin(), nums.end());
    int maxCount = 0;

    for (int num : nums) {
        if (numSet.find(num - 1) == numSet.end()) {  // Check if it's the start of a sequence
            int currentNum = num;
            int count = 1;

            // Count consecutive numbers
            while (numSet.find(currentNum + 1) != numSet.end()) {
                currentNum++;
                count++;
            }

            maxCount = max(maxCount, count);
        }
    }

    return maxCount;
}
```

# 24. Set Matrix Zeroes

## Problem Link

[LeetCode - Set Matrix Zeroes](https://leetcode.com/problems/set-matrix-zeroes/)

## Approach 1: Brute Force Approach

### Idea

In this approach, we use two nested loops to traverse every element of the matrix. If any element is `0`, we mark all the cells in the respective row and column with `-1` (or any other number that does not conflict with existing matrix values), except for the cells that originally contain `0`. After marking, we replace all marked cells with `0`.

This ensures that all rows and columns containing a `0` are set to zero, as required.

### Steps:

1. Traverse the matrix and whenever you find a `0`, mark all cells in the corresponding row and column with `-1` (except for those already containing `0`).
2. After the traversal, go through the matrix again and replace all `-1` values with `0`.

### Time Complexity

- **Time Complexity:** O(n \* m²)  
  We traverse the matrix for each element and mark the entire row and column, leading to quadratic time complexity.

### Space Complexity

- **Space Complexity:** O(1)  
  We are only using extra variables to mark the matrix, and no additional data structures are required.

### Code

```cpp
void setZeroes(vector<vector<int>> &matrix) {
    int n = matrix.size();        // number of rows
    int m = matrix[0].size();     // number of columns

    // First pass: mark rows and columns with -1
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            if (matrix[i][j] == 0) {
                // Mark all elements in the row with -1 (except those already 0)
                for (int x = 0; x < m; x++) {
                    if (matrix[i][x] != 0) matrix[i][x] = -1;
                }
                // Mark all elements in the column with -1 (except those already 0)
                for (int y = 0; y < n; y++) {
                    if (matrix[y][j] != 0) matrix[y][j] = -1;
                }
            }
        }
    }

    // Second pass: replace all -1 with 0
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            if (matrix[i][j] == -1) {
                matrix[i][j] = 0;
            }
        }
    }
}
```

## Approach 2: Using Two Extra Arrays

### Idea

In this approach, we use two additional arrays, one for tracking rows and another for tracking columns. If a cell contains `0`, we mark the corresponding row and column in these arrays. After the traversal, we iterate over the matrix again and set the corresponding cells to `0` based on the values in the row and column arrays.

### Steps:

1. Initialize two arrays, `row` and `col`, to keep track of rows and columns that need to be set to zero.
   - `row[i] = 1` means the entire row `i` should be set to zero.
   - `col[j] = 1` means the entire column `j` should be set to zero.
2. Traverse the matrix and update `row` and `col` arrays for any cell `(i, j)` that contains `0`.

3. Traverse the matrix again and set the cells `(i, j)` to zero if `row[i] == 1` or `col[j] == 1`.

### Time Complexity

- **Time Complexity:** O(n \* m)  
  We traverse the matrix twice, so the time complexity is linear with respect to the size of the matrix.

### Space Complexity

- **Space Complexity:** O(n + m)  
  We use two arrays of size `n` and `m` to store the row and column information.

### Code

```cpp
void setZeroes(vector<vector<int>> &matrix) {
    int n = matrix.size();        // number of rows
    int m = matrix[0].size();     // number of columns

    vector<int> row(n, 0);        // Array to track rows to be set to 0
    vector<int> col(m, 0);        // Array to track columns to be set to 0

    // First pass: Mark the rows and columns
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            if (matrix[i][j] == 0) {
                row[i] = 1;       // Mark row i to be set to 0
                col[j] = 1;       // Mark column j to be set to 0
            }
        }
    }

    // Second pass: Set matrix cells to 0 based on row and col arrays
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            if (row[i] == 1 || col[j] == 1) {
                matrix[i][j] = 0;
            }
        }
    }
}
```

### Approach 3: Optimized Space Complexity

**Intuition:**  
This approach uses the first row and first column of the matrix itself to keep track of which rows and columns need to be set to `0`. This optimization reduces the space complexity from O(N + M) to O(1) since we are not using any additional arrays.

#### Steps:

1. **Initialization:**  
   Declare a variable `col0` to keep track of whether the first column needs to be set to `0`. Initialize it to `1`.

2. **Mark Rows and Columns:**  
   Traverse the matrix:

   - If a cell `(i, j)` contains `0`, mark the first row and the first column:
     - Set `matrix[i][0]` to `0` for the `i-th` row.
     - Set `matrix[0][j]` to `0` for the `j-th` column.
   - If `i` is `0`, mark `matrix[0][0]` as `0`. If `j` is `0`, set `col0` to `0`.

3. **Set Matrix to Zero:**  
   Modify the matrix based on the markers:

   - For each cell `(i, j)` from `(1, 1)` to `(n-1, m-1)`, set `matrix[i][j]` to `0` if either `matrix[i][0]` or `matrix[0][j]` is `0`.

4. **Update the First Row and Column:**
   - If `matrix[0][0]` is `0`, set all elements in the first row to `0`.
   - If `col0` is `0`, set all elements in the first column to `0`.

#### Code Implementation:

```cpp
void setZeroes(vector<vector<int>>& matrix) {
    int n = matrix.size();
    int m = matrix[0].size();
    int col0 = 1;

    // Step 2: Mark the first row and column
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            if (matrix[i][j] == 0) {
                if (i == 0) {
                    matrix[0][0] = 0;
                } else {
                    matrix[i][0] = 0;
                }
                if (j == 0) {
                    col0 = 0;
                } else {
                    matrix[0][j] = 0;
                }
            }
        }
    }

    // Step 3: Set matrix to zero
    for (int i = 1; i < n; i++) {
        for (int j = 1; j < m; j++) {
            if (matrix[i][0] == 0 || matrix[0][j] == 0) {
                matrix[i][j] = 0;
            }
        }
    }

    // Step 4: Update the first row and column
    if (matrix[0][0] == 0) {
        for (int j = 0; j < m; j++) {
            matrix[0][j] = 0;
        }
    }
    if (col0 == 0) {
        for (int i = 0; i < n; i++) {
            matrix[i][0] = 0;
        }
    }
}
```

### Why Approach 1 Fails for Some Test Cases

**Approach 1** marks rows and columns with `-1` when a `0` is encountered, which can lead to incorrect modifications in matrices that contain negative values. This is because legitimate negative numbers can be misinterpreted as markers, causing false zeroing of elements.

#### Problematic Test Cases:

1. **General Case:**

   - Matrix: `[[0, 1, -2], [3, 4, -1], [2, -3, 5]]`
   - Reason for Failure: Negative values (-2, -1, -3) exist in the matrix. When `0` is found, the approach marks the corresponding row and column with `-1`, interfering with the legitimate negative values and leading to incorrect results.

2. **Specific Case:**
   - Matrix: `[[0, -1, 2], [3, 0, 5], [1, -2, 3]]`
   - Reason for Failure: The presence of negative numbers during the marking process will cause erroneous modifications when setting rows and columns to `0`.

#### Conclusion:

Approach 1 is unsuitable for matrices that contain negative numbers, as the marking strategy conflicts with legitimate values, resulting in incorrect outcomes.

# 25. Rotate Image

## Problem Link

[LeetCode - Rotate Image](https://leetcode.com/problems/rotate-image/description/)

### Problem Statement

Given an n x n 2D matrix representing an image, rotate the image by 90 degrees (clockwise). You have to rotate the matrix in place, which means you need to modify the input 2D matrix directly.

### Approach 1: Reverse Columns

**Idea**:

- The basic idea is to reverse the columns of the matrix after reversing the rows. This effectively rotates the image by 90 degrees clockwise.
- This method involves creating a new matrix to store the rotated result temporarily.

**Steps**:

- Create a new matrix `ans` of the same size as the input `matrix`.
- Iterate through each column of the original matrix in reverse order.
- Assign the values to the new matrix from the original matrix in a transposed manner.

**Time Complexity**: O(n²)

- The function has to visit each element in the matrix, leading to quadratic time complexity.

**Space Complexity**: O(n²)

- A new matrix of the same size is created, resulting in quadratic space usage.

3. **Code**:

   ```cpp
   void rotate(vector<vector<int>>& matrix) {
       vector<vector<int>> ans(matrix.size(), vector<int>(matrix[0].size()));
       int row = matrix.size();
       int col = matrix[0].size();
       int rs = 0, re = row - 1, cs = 0, ce = col - 1;
       int ansrow = 0, anscol = 0;

       for (int i = cs; i <= ce; i++) {
           for (int j = re; j >= 0; j--) {
               ans[ansrow][anscol] = matrix[j][i];
               anscol++;
           }
           ansrow++;
           anscol = 0; // Reset anscol for the next column
       }

       // Copy the new matrix back to the original matrix
       matrix = ans;
   }
   ```

### Approach 2: Transpose and Swap Columns

**Idea**:

- This approach involves two main steps: first, transposing the matrix, then swapping the columns to achieve the desired rotation.
- Transposing involves swapping elements at positions (i, j) with (j, i), and swapping the columns helps achieve the clockwise rotation.

**Steps**:

- **Step 1**: Transpose the matrix:
  - Iterate through the upper triangle of the matrix and swap elements to transpose it.
- **Step 2**: Swap the columns:
  - Swap the first column with the last, the second column with the second last, and so on, until reaching the middle of the matrix.

**Time Complexity**: O(n²)

- This approach also requires visiting every element in the matrix twice (once for transposition and once for column swapping), resulting in quadratic time complexity.

**Space Complexity**: O(1)

- This approach does not require additional space for another matrix, thus using constant space.

  **Code**:

  ```cpp
  void rotate(vector<vector<int>>& matrix) {
      // Step 1: Transpose the matrix
      int n = matrix.size();
      for (int i = 0; i < n; i++) {
          for (int j = 0; j < n; j++) {
              if (i < j) {
                  swap(matrix[i][j], matrix[j][i]);
              }
          }
      }

      // Step 2: Swap the columns
      int scol = 0;
      int ecol = n - 1;
      while (scol < ecol) {
          for (int row = 0; row < n; row++) {
              swap(matrix[row][scol], matrix[row][ecol]);
          }
          scol++;
          ecol--;
      }
  }
  ```

---

### Conclusion

- **Approach 1** is straightforward but requires additional space for a new matrix.
- **Approach 2** is more efficient in terms of space and modifies the matrix in place, making it preferable for larger matrices.

This detailed breakdown can help anyone understand the problem and the different approaches to solving it effectively. Feel free to add it to your GitHub repository!
