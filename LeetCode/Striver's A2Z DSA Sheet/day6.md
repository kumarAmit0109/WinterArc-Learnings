# 26. Spiral Matrix

### Problem Link

[Leetcode - Spiral Matrix](https://leetcode.com/problems/spiral-matrix/description/)

### Approach

1. **Initialize** a result vector `ans` to store the elements in spiral order.
2. **Define** the following variables:
   - `row`: number of rows in the matrix.
   - `col`: number of columns in the matrix.
   - `count`: a counter to keep track of the number of elements processed.
   - `total`: the total number of elements in the matrix (`row * col`).
3. **Initialize** the boundary indices:
   - `rs` (row start), `re` (row end), `cs` (column start), `ce` (column end).
4. **Traverse** the matrix in a spiral order:
   - Move from left to right across the starting row.
   - Move from top to bottom down the last column.
   - Move from right to left across the last row.
   - Move from bottom to top up the first column.
5. **Repeat** the process, narrowing the boundaries, until all elements are processed.
6. **Return** the result vector `ans`.

### Time and Space Complexity

- **Time Complexity**: `O(m * n)`  
  The function processes each element of the matrix once, where `m` is the number of rows and `n` is the number of columns.
- **Space Complexity**: `O(1)` (excluding the space used for the result vector).

### Code

```cpp
vector<int> spiralOrder(vector<vector<int>>& matrix) {
    vector<int> ans;
    int row = matrix.size();
    int col = matrix[0].size();
    int count = 0;
    int total = row * col;

    // Index initialization
    int rs = 0, re = row - 1;
    int cs = 0, ce = col - 1;

    while (count < total) {
        // Printing starting row
        for (int i = cs; count < total && i <= ce; i++) {
            ans.push_back(matrix[rs][i]);
            count++;
        }
        rs++;

        // Printing last column
        for (int i = rs; count < total && i <= re; i++) {
            ans.push_back(matrix[i][ce]);
            count++;
        }
        ce--;

        // Printing last row
        for (int i = ce; count < total && i >= cs; i--) {
            ans.push_back(matrix[re][i]);
            count++;
        }
        re--;

        // Printing first column
        for (int i = re; count < total && i >= rs; i--) {
            ans.push_back(matrix[i][cs]);
            count++;
        }
        cs++;
    }

    return ans;
}
```

# 27. Subarray Sum Equals K

### Problem Link

[Leetcode - Subarray Sum Equals K](https://leetcode.com/problems/subarray-sum-equals-k/description/)

## Approach 1: Brute Force (Nested Loops)

1. **Idea**: Generate all possible subarrays using two nested loops. For each subarray, calculate its sum and check if it equals `k`. If the sum matches `k`, increment the count.
2. **Steps**:

   - Use two loops to generate every possible subarray.
   - For each subarray, compute the sum.
   - If the sum equals `k`, increment the count.

3. **Time Complexity**: `O(n^2)`  
   Generating all subarrays takes `O(n^2)` where `n` is the size of the array.

4. **Space Complexity**: `O(1)`  
   No extra space is used apart from variables for sum and count.

### Code

```cpp
int subarraySum(vector<int>& nums, int k) {
    int count = 0;

    for (int i = 0; i < nums.size(); i++) {
        int sum = 0;
        for (int j = i; j < nums.size(); j++) {
            sum += nums[j];
            if (sum == k) {
                count++;
            }
        }
    }

    return count;
}
```

## Approach 2: Prefix Sum with HashMap

1. **Idea**: Use a hash map to store the frequency of prefix sums encountered. While traversing the array, compute the prefix sum and check if `(prefix sum - k)` exists in the map. If it exists, it means a subarray with sum `k` has been found.
2. **Steps**:

   - Initialize a variable `sum` to store the running prefix sum and a hash map to keep track of the prefix sums encountered so far.
   - For each element in the array, update the prefix sum and check if `(sum - k)` exists in the map.
   - If it exists, add the frequency of `(sum - k)` to the count.
   - Finally, update the frequency of the current prefix sum in the map.

3. **Time Complexity**: `O(n)`  
   The array is traversed once, and map operations are constant time on average.

4. **Space Complexity**: `O(n)`  
   The space complexity is proportional to the size of the map.

### Code

```cpp
int subarraySum(vector<int>& nums, int k) {
    unordered_map<int, int> prefixSumMap;
    int sum = 0, count = 0;
    prefixSumMap[0] = 1;  // Base case to handle subarray sum equals k from the start

    for (int i = 0; i < nums.size(); i++) {
        sum += nums[i];

        if (prefixSumMap.find(sum - k) != prefixSumMap.end()) {
            count += prefixSumMap[sum - k];
        }

        prefixSumMap[sum]++;
    }

    return count;
}
```

# 28. Pascal's Triangle

### Problem Link

[Leetcode - Pascal's Triangle](https://leetcode.com/problems/pascals-triangle/description/)

### References

- [Math Centre - Pascal's Triangle Explanation](https://www.mathcentre.ac.uk/resources/uploaded/mc-ty-pascal-2009-1.pdf)
- [TakeUForward - Pascal's Triangle](https://takeuforward.org/data-structure/program-to-generate-pascals-triangle/)
- [Video Solution](https://youtu.be/jC0wWjBrKss?si=pn8v6_UlWPnfZ_JY)

## Approach: Iterative Construction

1. **Idea**: We construct Pascal's Triangle row by row. Each row starts and ends with 1. Every element in between is the sum of the two elements directly above it in the previous row.
2. **Steps**:

   - Create a 2D vector `result` where each row represents a row in Pascal's Triangle.
   - For each row `i`, initialize the row with `i + 1` elements, all set to 1.
   - For each element at position `j` in row `i`, update it with the sum of the elements at positions `j - 1` and `j` from the previous row.
   - Continue this process for all rows up to `numRows`.

3. **Time Complexity**: `O(numRows^2)`  
   Each row has at most `numRows` elements, and we iterate over all elements to fill in the triangle.

4. **Space Complexity**: `O(numRows^2)`  
   The space complexity is proportional to the size of the 2D vector that stores the entire triangle.

### Code

```cpp
vector<vector<int>> generate(int numRows) {
    vector<vector<int>> result(numRows);

    for (int i = 0; i < numRows; i++) {
        result[i] = vector<int>(i + 1, 1);

        for (int j = 1; j < i; j++) {
            result[i][j] = result[i - 1][j - 1] + result[i - 1][j];
        }
    }

    return result;
}
```
