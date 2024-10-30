# 126. Subset Sum

### Problem Link

[Subset Sum - Naukri Code360](https://www.naukri.com/code360/problems/subset-sum_630213)

## Approach: Recursive Backtracking

### Algorithm

1. **Recursive Helper Function**:
   - Use a helper function that keeps track of the current index and the cumulative sum.
   - At each index, decide to either include the current element in the sum or exclude it.
2. **Base Cases**:
   - If the current sum equals the target \( k \), return `true`.
   - If the index exceeds the array size, return `false`.
3. **Combine Results**:
   - Return `true` if either including or excluding the element at the current index leads to a subset with the sum equal to \( k \).

### Time Complexity

- Since each element can either be included or excluded, the time complexity is \( O(2^n) \).

### Space Complexity

- The space complexity is \( O(n) \) for the recursive stack, where \( n \) is the number of elements.

### Code

```cpp
#include <iostream>
#include <vector>
using namespace std;

bool isSubsetPresentHelper(int i, int n, int k, vector<int> &a, int currentSum) {
    if (currentSum == k) {
        return true;
    }
    if (i >= n) {
        return false;
    }

    bool include = isSubsetPresentHelper(i + 1, n, k, a, currentSum + a[i]);
    bool exclude = isSubsetPresentHelper(i + 1, n, k, a, currentSum);

    return include || exclude;
}

bool isSubsetPresent(int n, int k, vector<int> &a) {
    return isSubsetPresentHelper(0, n, k, a, 0);
}
```

# 127. Combination Sum

### Problem Link

[Combination Sum - LeetCode](https://leetcode.com/problems/combination-sum/description/)

## Approach: Recursive Backtracking with Backtracking

### Algorithm

1. **Recursive Step**:
   - For each element in the array, consider it as part of the current combination.
   - Recursively call for the remaining target (`target - arr[i]`).
   - After the recursive call returns, backtrack by removing the current element from the current combination.
2. **Base Cases**:
   - If `target` becomes zero, the current combination is valid; add it to the result.
   - If the target is still positive but there are no more elements left to consider, return to explore other combinations.

### Time Complexity

- Given that each element can be picked multiple times, the complexity is approximately \( O(2^n ) \).

### Space Complexity

- Space complexity is \( O (n) \), due to recursive stack depth for each path.

### Code

```cpp
void findCombinations(vector<int>& candidates, int target, vector<int>& current, vector<vector<int>>& result, int start) {
    if (target == 0) {
        result.push_back(current);
        return;
    }
    if (target < 0) {
        return;
    }

    for (int i = start; i < candidates.size(); i++) {
        current.push_back(candidates[i]);
        findCombinations(candidates, target - candidates[i], current, result, i);
        current.pop_back();  // Backtrack
    }
}

vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
    vector<vector<int>> result;
    vector<int> current;
    findCombinations(candidates, target, current, result, 0);
    return result;
}
```

# 128. Combination Sum II

### Problem Link

[Combination Sum II - LeetCode](https://leetcode.com/problems/combination-sum-ii/description/)

## Approach: Recursive Backtracking with Element Skipping for Duplicates

### Algorithm

1. **Recursive Calls**:
   - For each element, make two recursive calls:
     - **Include** the current element in the combination and call with `target - element`.
     - **Exclude** the current element and proceed with the original target.
   - Skip any duplicates in the array to avoid duplicate combinations in the result.
2. **Base Cases**:
   - If `target == 0`, the required combination is found; push it to the result array.
   - If the array has been completely traversed but the target is still greater than 0, no valid combination exists.

### Time Complexity

- Due to skipping duplicates and limiting combinations, time complexity is approximately \( O(2^n) \).

### Space Complexity

- Space complexity is \( O(n) \), for the recursive stack depth.

### Code

```cpp

void findCombinations2(int start, vector<int>& candidates, int target, vector<int>& current, vector<vector<int>>& result) {
    if (target == 0) {
        result.push_back(current);
        return;
    }
    if (target < 0) {
        return;
    }

    for (int i = start; i < candidates.size(); i++) {
        if (i > start && candidates[i] == candidates[i - 1]) continue;  // Skip duplicates

        current.push_back(candidates[i]);
        findCombinations2(i + 1, candidates, target - candidates[i], current, result);
        current.pop_back();  // Backtrack
    }
}

vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
    sort(candidates.begin(), candidates.end());
    vector<vector<int>> result;
    vector<int> current;
    findCombinations2(0, candidates, target, current, result);
    return result;
}
```

# 129. Subset Sums

### Problem Link

[Subset Sums - GeeksforGeeks](https://www.geeksforgeeks.org/problems/subset-sums2234/1)

## Approach: Recursive Backtracking for Subset Sums

### Algorithm

1. **Recursive Calls**:
   - At each step, make two recursive calls:
     - **Include** the current element in the subset sum.
     - **Exclude** the current element from the subset sum.
   - When the recursive calls return, all possible subset sums are collected in the `ans` vector.
2. **Base Case**:

   - When the `index` exceeds the size of the array, add the current subset sum to the `ans` vector.

3. **Sorting**:
   - After all recursive calls are complete, sort `ans` to return subset sums in ascending order.

### Time Complexity

- **O(2^n)**, where \(n\) is the size of the array, due to generating all possible subsets.

### Space Complexity

- **O(2^n)**, to store all subset sums in the `ans` vector.

### Code

```cpp
void calculateSubsetSums(int index, int currentSum, vector<int>& arr, int n, vector<int>& ans) {
    // Base case: if index exceeds the size of the array, store the current sum
    if (index == n) {
        ans.push_back(currentSum);
        return;
    }

    // Recursive call including the current element in the sum
    calculateSubsetSums(index + 1, currentSum + arr[index], arr, n, ans);

    // Recursive call excluding the current element from the sum
    calculateSubsetSums(index + 1, currentSum, arr, n, ans);
}

vector<int> subsetSums(vector<int> arr, int n) {
    vector<int> ans;

    // Initial call to the helper function starting from index 0 and sum 0
    calculateSubsetSums(0, 0, arr, n, ans);

    // Sort the result to have subset sums in ascending order
    sort(ans.begin(), ans.end());
    return ans;
}
```

# 130. Subsets II

### Problem Link

[Subsets II - LeetCode](https://leetcode.com/problems/subsets-ii/description/)

## Approach 1: Recursive Backtracking with Set for Unique Subsets

### Algorithm

1. **Recursive Calls**:
   - For each element at an index, we make two recursive calls:
     - **Include** the current element and proceed to the next index.
     - **Exclude** the current element and proceed to the next index.
   - After the recursive calls, store the subset in a set to automatically handle duplicates.
2. **Base Case**:
   - When the end of the array is reached, add the current subset to the set.
3. **Set for Unique Subsets**:
   - Using a set to store subsets ensures all duplicates are automatically discarded.

### Time Complexity

- \(O(2^n times k)\), where \(n\) is the number of elements in `nums`, and \(k\) is the average length of subsets generated, due to the exponential growth in subsets.

### Space Complexity

- \(O(2^n times k)\), for storing subsets in the result and the set.

### Code

```cpp
void generateSubsetsWithSet(int index, vector<int>& nums, vector<int>& currentSubset, set<vector<int>>& uniqueSubsets) {
    if (index == nums.size()) {
        uniqueSubsets.insert(currentSubset);
        return;
    }

    // Include the current element
    currentSubset.push_back(nums[index]);
    generateSubsetsWithSet(index + 1, nums, currentSubset, uniqueSubsets);
    currentSubset.pop_back();

    // Exclude the current element
    generateSubsetsWithSet(index + 1, nums, currentSubset, uniqueSubsets);
}

vector<vector<int>> subsetsWithDup(vector<int>& nums) {
    set<vector<int>> uniqueSubsets;  // Set to store unique subsets
    vector<vector<int>> result;
    vector<int> currentSubset;

    // Sort the input array to handle duplicates
    sort(nums.begin(), nums.end());

    // Generate all subsets and store unique ones in the set
    generateSubsetsWithSet(0, nums, currentSubset, uniqueSubsets);

    // Transfer unique subsets from set to result vector
    result.assign(uniqueSubsets.begin(), uniqueSubsets.end());

    return result;
}
```

## Approach: Recursive Backtracking with Duplicate Skipping

### Algorithm

1. **Sorting**:
   - Sort the input array to bring duplicate elements together, making it easier to skip duplicates during the recursion.
2. **Backtracking**:
   - For each element, make a recursive call to include it in the subset, and another call to exclude it.
   - Before including an element, check if it’s a duplicate of the previous element at the same recursion level. If it is, skip it to avoid duplicate subsets.
3. **Result Storage**:
   - Append the current subset to the result list at each recursive level, and backtrack by removing the last element after each recursive call.

### Time Complexity

1. **Sorting the Input:**

   - The input array `nums` is sorted initially, which takes \(O(n \log n)\), where \(n\) is the size of the input array.

2. **Backtracking to Generate Subsets:**

   - The `findSubsetsWithBacktracking` function generates all possible subsets. The number of subsets of a set with \(n\) elements is \(2^n\).
   - However, since there are duplicates in `nums`, the actual number of unique subsets will be less than \(2^n\), but in the worst case (when all elements are unique), we can still consider this contribution to be \(O(2^n)\).

3. **Recursion and Processing:**
   - Each recursive call processes one subset and may take additional \(O(n)\) time to copy the current subset to the results. Hence, while the backtracking itself is \(O(2^n)\), the copy operation adds a factor of \(O(n)\), leading to a total complexity of \(O(n times 2^n)\) for generating subsets.

Overall, combining these components:

- **Total Time Complexity:**

  O(n \log n + n times 2^n) (dominated by the backtracking)

### Space Complexity

1. **Space for Result Storage:**

   - The result vector stores all subsets. In the worst case, the space required for storing subsets will also be \(O(2^n)\).

2. **Recursion Stack Space:**
   - The maximum depth of the recursion stack can go up to \(n\), leading to an additional space requirement of \(O(n)\).

Combining these:

- **Total Space Complexity:**
  O(2^n + n) (dominated by the result storage)

### Code

```cpp
void findSubsetsWithBacktracking(int index, vector<int>& nums, vector<int>& currentSubset, vector<vector<int>>& result) {
    result.push_back(currentSubset);

    for (int i = index; i < nums.size(); i++) {
        // Skip duplicates
        if (i > index && nums[i] == nums[i - 1]) continue;

        // Include the current element
        currentSubset.push_back(nums[i]);

        // Recurse to find further subsets
        findSubsetsWithBacktracking(i + 1, nums, currentSubset, result);

        // Backtrack by removing the last element
        currentSubset.pop_back();
    }
}

vector<vector<int>> subsetsWithDup(vector<int>& nums) {
    vector<vector<int>> result;
    vector<int> currentSubset;

    // Sort input array to handle duplicates
    sort(nums.begin(), nums.end());

    // Start backtracking from the 0th index
    findSubsetsWithBacktracking(0, nums, currentSubset, result);

    return result;
}
```
