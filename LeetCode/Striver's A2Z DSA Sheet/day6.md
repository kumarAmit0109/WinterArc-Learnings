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

# 29. Majority Element II

### Problem Link

[Leetcode - Majority Element II](https://leetcode.com/problems/majority-element-ii/description/)

## Approach 1: Brute Force (Count Elements)

1. **Idea**: For each element in the array, count its frequency. If the frequency of any element is greater than `nums.size() / 3`, add it to the result if it's not already in the result array.
2. **Steps**:

   - For every element, count how many times it appears in the array.
   - If an element's count exceeds `nums.size() / 3`, check if it's already in the result.
   - If not, add it to the result array.
   - Return the result.

3. **Time Complexity**: `O(n^2)`  
   For each element, we traverse the array to count its occurrences.

4. **Space Complexity**: `O(n)`  
   Space is used to store the result array.

### Code

```cpp
vector<int> majorityElement(vector<int>& nums) {
    vector<int> ans;
    int n = nums.size();

    for (int i = 0; i < n; i++) {
        int count = 0;

        // Count frequency of nums[i]
        for (int j = 0; j < n; j++) {
            if (nums[i] == nums[j]) {
                count++;
            }
        }

        // Check if count is more than n/3 and element is not already in the result
        if (count > n / 3 && find(ans.begin(), ans.end(), nums[i]) == ans.end()) {
            ans.push_back(nums[i]);
        }
    }

    return ans;
}
```

## Approach 2: HashMap (Frequency Count)

1. **Idea**: We maintain a frequency count for each element using a hash map. After counting, we check which elements have a frequency greater than `nums.size() / 3`.

2. **Steps**:

   - Traverse the array and use a hash map to store the frequency of each element.
   - After populating the map, check for elements whose frequency exceeds `nums.size() / 3`.
   - Add these elements to the result vector and return the result.

3. **Time Complexity**: `O(n)`  
   We traverse the array once to build the frequency map and then again to collect elements with a frequency greater than `n/3`.

4. **Space Complexity**: `O(n)`  
   The hash map uses extra space proportional to the number of unique elements.

### Code

```cpp
vector<int> majorityElement(vector<int>& nums) {
    unordered_map<int, int> freqMap;
    vector<int> result;
    int n = nums.size();

    // Step 1: Count frequency of each element
    for (int num : nums) {
        freqMap[num]++;
    }

    // Step 2: Find elements with frequency > n/3
    for (auto& [num, count] : freqMap) {
        if (count > n / 3) {
            result.push_back(num);
        }
    }

    return result;
}
```

## Approach 3: Boyer-Moore Voting Algorithm

1. **Idea**: Since at most two elements can have a frequency greater than `nums.size() / 3`, we maintain two candidates (`elem1`, `elem2`) and their respective counts (`count1`, `count2`). We then make a second pass to validate the candidates.

2. **Steps**:

   - Traverse the array to find two potential majority elements using the Boyer-Moore Voting Algorithm.
   - On the second pass, validate if these candidates appear more than `nums.size() / 3` times.

3. **Conditions**:

   1. If `count1 == 0` and `nums[i] != elem2`, update `elem1` to `nums[i]` and set `count1 = 1`.
   2. If `count2 == 0` and `nums[i] != elem1`, update `elem2` to `nums[i]` and set `count2 = 1`.
   3. If `nums[i] == elem1`, increment `count1`.
   4. If `nums[i] == elem2`, increment `count2`.
   5. Else, decrement `count1` and `count2`.

4. **Time Complexity**: `O(n)`  
   We traverse the array twice: once to find candidates and once to validate them.

5. **Space Complexity**: `O(1)`  
   The algorithm uses a constant amount of extra space for variables `elem1`, `elem2`, `count1`, and `count2`.

### Code

```cpp
vector<int> majorityElement(vector<int>& nums) {
    int elem1 = -1, elem2 = -1, count1 = 0, count2 = 0;
    int n = nums.size();

    // Step 1: Find potential candidates using Boyer-Moore Voting Algorithm
    for (int num : nums) {
        if (num == elem1) {
            count1++;
        } else if (num == elem2) {
            count2++;
        } else if (count1 == 0) {
            elem1 = num;
            count1 = 1;
        } else if (count2 == 0) {
            elem2 = num;
            count2 = 1;
        } else {
            count1--;
            count2--;
        }
    }

    // Step 2: Validate the candidates
    count1 = count2 = 0;
    for (int num : nums) {
        if (num == elem1) count1++;
        else if (num == elem2) count2++;
    }

    vector<int> result;
    if (count1 > n / 3) result.push_back(elem1);
    if (count2 > n / 3) result.push_back(elem2);

    return result;
}
```

# 30. 3Sum

### Problem Link

[Leetcode - 3Sum](https://leetcode.com/problems/3sum/description/)

## Approach 1: Brute Force with Three Nested Loops

### Steps:

1. **Declare** a set `uniqueTriplets` to store the unique triplets.
2. **Iterate** over the array with three nested loops:
   - The outer loop runs with index `i` from `0` to `n-2`.
   - The middle loop runs with index `j` from `i+1` to `n-1`.
   - The inner loop runs with index `k` from `j+1` to `n-1`.
3. **Check** the sum of the current triplet `nums[i] + nums[j] + nums[k]`:
   - If the sum is equal to 0, sort the triplet and insert it into the set `uniqueTriplets`.
4. **Convert** the set of triplets to a vector of vectors and return it as the result.

### Time and Space Complexity

- **Time Complexity**: `O(n^3)`  
  The three nested loops result in a cubic time complexity.
- **Space Complexity**: `O(m)`  
  The space complexity depends on the number of unique triplets `m` stored in the set.

### Code

```cpp
vector<vector<int>> threeSum(vector<int>& nums) {
    set<vector<int>> uniqueTriplets;  // To store unique triplets

    int n = nums.size();

    // Iterate over all possible triplets using three nested loops
    for (int i = 0; i < n - 2; i++) {
        for (int j = i + 1; j < n - 1; j++) {
            for (int k = j + 1; k < n; k++) {
                // Check if the sum of the triplet is zero
                if (nums[i] + nums[j] + nums[k] == 0) {
                    vector<int> triplet = {nums[i], nums[j], nums[k]};
                    sort(triplet.begin(), triplet.end());  // Sort the triplet
                    uniqueTriplets.insert(triplet);  // Insert into set to avoid duplicates
                }
            }
        }
    }

    // Convert the set to a vector of vectors and return it
    return vector<vector<int>>(uniqueTriplets.begin(), uniqueTriplets.end());
}
```

## Approach 2: HashSet to Find the Third Element

### Steps:

1. **Declare** a set `uniqueTriplets` to store the unique triplets.
2. Use an outer loop with index `i` running from `0` to `n-1` to fix the first element.
3. **Declare** a HashSet `seen` to keep track of elements between indices `i` and `j`.
4. Use an inner loop with index `j` running from `i+1` to `n-1` to iterate over the array for the second element.
5. **Calculate** the value of the third element as `-(nums[i] + nums[j])`.
6. **Check** if this third element exists in the HashSet:
   - If it exists, sort the triplet and insert it into the set `uniqueTriplets`.
7. After processing the inner loop, **insert** the current element `nums[j]` into the HashSet.
8. **Convert** the set of triplets to a vector of vectors and return it as the result.

### Time and Space Complexity

- **Time Complexity**: `O(n^2)`  
  We use two loops, and each loop runs in `O(n)`. The time complexity is quadratic, which is an improvement from the cubic approach.

- **Space Complexity**: `O(n)`  
  The space complexity is linear because of the HashSet used to track the elements and the space used for storing the unique triplets.

### Code

```cpp
vector<vector<int>> threeSum(vector<int>& nums) {
    set<vector<int>> uniqueTriplets;  // To store unique triplets
    int n = nums.size();

    // Iterate over each element as the first fixed element
    for (int i = 0; i < n - 1; i++) {
        unordered_set<int> seen;  // HashSet to track the second element
        for (int j = i + 1; j < n; j++) {
            int thirdElement = -(nums[i] + nums[j]);  // The third element we are looking for
            if (seen.find(thirdElement) != seen.end()) {
                vector<int> triplet = {nums[i], nums[j], thirdElement};
                sort(triplet.begin(), triplet.end());  // Sort the triplet
                uniqueTriplets.insert(triplet);  // Insert into set to avoid duplicates
            }
            seen.insert(nums[j]);  // Add current element to the set
        }
    }

    // Convert the set to a vector of vectors and return it
    return vector<vector<int>>(uniqueTriplets.begin(), uniqueTriplets.end());
}
```

## Approach 3: Two Pointers after Sorting

### Steps:

1. **Sort** the input array `nums` to enable the use of two pointers.
2. Use an outer loop with index `i` running from `0` to `n-1` to fix the first element:
   - **Skip** duplicates by checking if the current element is the same as the previous one.
3. Initialize two pointers:
   - `j` starting from `i + 1` (the next element).
   - `k` starting from the last index of the array.
4. While `j` is less than `k`, calculate the sum of the triplet `nums[i] + nums[j] + nums[k]`:
   - If the sum is **greater than** `0`, decrement the pointer `k` to reduce the sum.
   - If the sum is **less than** `0`, increment the pointer `j` to increase the sum.
   - If the sum is **equal to** `0`, store the triplet in the result, then:
     - Increment `j` and decrement `k`, skipping duplicates.
5. After processing, continue to the next iteration of the outer loop.
6. **Return** the list of unique triplets.

### Time and Space Complexity

- **Time Complexity**: `O(n^2)`  
  The outer loop runs in `O(n)`, and the inner loop also runs in `O(n)` for each iteration of the outer loop.

- **Space Complexity**: `O(1)`  
  We are using constant space for pointers and the result, not counting the output.

### Code

```cpp
vector<vector<int>> threeSum(vector<int>& nums) {
    set<vector<int>> uniqueTriplets;  // To store unique triplets
    int n = nums.size();
    sort(nums.begin(), nums.end());  // Sort the array

    // Iterate over each element as the first fixed element
    for (int i = 0; i < n - 2; i++) {
        if (i > 0 && nums[i] == nums[i - 1]) continue;  // Skip duplicates

        int j = i + 1;  // Start second pointer
        int k = n - 1;  // Start third pointer

        while (j < k) {
            int sum = nums[i] + nums[j] + nums[k];
            if (sum > 0) {
                k--;  // We need a smaller sum
            } else if (sum < 0) {
                j++;  // We need a larger sum
            } else {
                uniqueTriplets.insert({nums[i], nums[j], nums[k]});  // Found a triplet
                j++;
                k--;
                // Skip duplicates for j
                while (j < k && nums[j] == nums[j - 1]) j++;
                // Skip duplicates for k
                while (j < k && nums[k] == nums[k + 1]) k--;
            }
        }
    }

    // Convert the set to a vector of vectors and return it
    return vector<vector<int>>(uniqueTriplets.begin(), uniqueTriplets.end());
}
```
