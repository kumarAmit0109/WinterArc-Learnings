# 121. Binary Strings with No Consecutive 1s

### Problem Link

[Naukri Code360 - Binary Strings with No Consecutive 1s](https://www.naukri.com/code360/problems/binary-strings-with-no-consecutive-1s_893001)

## Approach: Recursive Generation with Constraints

### Algorithm

1. **Recursive Calls**:
   - Start with an empty string and make two recursive calls at each step.
   - First, append `'0'` to the current string and make a recursive call.
   - Second, append `'1'` to the current string only if the last character was not `'1'` to prevent consecutive `1`s.
2. **Base Case**:
   - When the length of the current string (`temp`) equals `N`, add it to the answer list `ans`.
3. **Result Collection**:
   - Collect all valid strings in a vector and return them.

### Time Complexity

- The recursive approach generates each valid string of length `N`, and since there are `O(2^N)` possible binary strings, the time complexity is approximately **O(2^N)** in the worst case.

### Space Complexity

- The recursion depth is `O(N)`, and the result storage is `O(2^N)` for all generated strings, leading to a space complexity of **O(2^N)**.

### Code

```cpp
#include <vector>
#include <string>
using namespace std;

void generateStringsHelper(int N, string temp, vector<string>& ans) {
    // Base case: if the current string length is equal to N
    if (temp.length() == N) {
        ans.push_back(temp);
        return;
    }

    // Recursive case 1: Include '0' and make a recursive call
    generateStringsHelper(N, temp + "0", ans);

    // Recursive case 2: Include '1' only if the last character is not '1'
    if (temp.empty() || temp.back() != '1') {
        generateStringsHelper(N, temp + "1", ans);
    }
}

vector<string> generateString(int N) {
    vector<string> ans;
    generateStringsHelper(N, "", ans);
    return ans;
}
```

# 122. Generate Parentheses

### Problem Link

[LeetCode - Generate Parentheses](https://leetcode.com/problems/generate-parentheses/)

## Approach: Recursive Backtracking with Left-Middle-Right Insertion

### Algorithm

1. **Initialize**: Start with an empty string and decide where to add parentheses at each step.
2. **Recursive Choices**:
   - Add an open parenthesis `(` if the count of open parentheses used is less than `n`.
   - Add a closing parenthesis `)` if the count of closing parentheses is less than the count of open parentheses used.
3. **Base Case**: When both open and close counts reach `n`, a valid combination is formed; add this to the result list.
4. **Avoid Duplicates**: Since the approach inherently tracks the valid count of open and close parentheses, no duplicates are generated.

### Time Complexity

- **Number of Valid Combinations**: The number of valid combinations of parentheses for \( n \) pairs is given by the nth Catalan number, which can be approximated as:

  C(n) = (1/n+1) \* combination (2n ,n) ~= 4^n / n^{3/2}

  Thus, the time complexity for generating all combinations is O(4^n / n) .

- **String Construction**: Each valid combination is a string of length \( 2n \). While constructing strings during recursion, the concatenation operation has a time complexity of \( O(n) \) in the worst case.

  Putting these together, the overall time complexity can be approximated as:

  O(n \* (4^n/ n)) = O(4^n/ sqrt(n))

### Space Complexity

- **Recursive Stack Space**: The maximum depth of recursion is \( O(n) \), as each level corresponds to a position for either an opening or closing parenthesis.
- **Storage for Results**: Since we generate all possible valid combinations, each of length \( 2n \), we need \( O(C(n) \* 2n) \) space, which approximates to \( O(n \* 4^n /sqrt(n) ) \).

### Code

```cpp
void generateParentheses(int open, int close, int n, string current, vector<string>& result) {
    // Base case: when current combination is valid and of length 2 * n
    if (open == n && close == n) {
        result.push_back(current);
        return;
    }
    // Recursively add pairs in three possible positions

    // Add open parenthesis if open count is less than n
    if (open < n) {
        generateParentheses(open + 1, close, n, current + "(", result);
    }

    // Add closing parenthesis if close count is less than open
    if (close < open) {
        generateParentheses(open, close + 1, n, current + ")", result);
    }
}

vector<string> generateParenthesis(int n) {
    vector<string> result;
    generateParentheses(0, 0, n, "", result);
    return result;
}
```

# 123. Subsets

### Problem Link

[LeetCode - Subsets](https://leetcode.com/problems/subsets/)

## Approach 1: Recursive Backtracking

### Algorithm

1. **Base Case**: If the current index reaches the end of the `nums` array, add the current subset (`temp`) to the result (`ans`).
2. **Recursive Choices**:
   - Include the current element at `index` in the subset.
   - Exclude the current element, backtracking to explore all possible subsets.
3. **Recursion & Backtracking**:
   - Push the current element and call the recursive function for the next index.
   - Pop the element to backtrack and explore the path without this element.

### Time Complexity

- Since each element can either be included or excluded, there are \( 2^n \) subsets for an input array of size \( n \).
- The recursion depth is \( n \), resulting in a time complexity of \( O(2^n) \).

### Space Complexity

- The space complexity is \( O(n) \) for the recursive stack.

### Code

```cpp
void solve(vector<int>& nums, vector<vector<int>>& ans, vector<int>& temp, int index) {
    if (index == nums.size()) {
        ans.push_back(temp);
        return;
    }
    // Include the current element
    temp.push_back(nums[index]);
    solve(nums, ans, temp, index + 1);
    // Backtrack to exclude the current element
    temp.pop_back();
    solve(nums, ans, temp, index + 1);
}

vector<vector<int>> subsets(vector<int>& nums) {
    vector<int> temp;
    vector<vector<int>> ans;
    int index = 0;
    solve(nums, ans, temp, index);
    return ans;
}
```

## Approach 2: Iterative Bit Manipulation

### Algorithm

1. **Initialization**: Calculate the total number of subsets as \( 2^n \), where \( n \) is the size of the input array `nums`.
2. **Loop Through Subsets**:
   - For each subset, use an integer \( i \) as a bitmask, iterating from \( 0 \) to \( 2^n - 1 \).
3. **Extract Elements Using Bits**:
   - For each bit in \( i \), check if it is set (1).
   - If set, include the corresponding element from `nums` in the subset.
   - Append the current subset to `ans`.

### Time Complexity

- There are \( 2^n \) possible subsets, and generating each subset takes \( O(n) \) time. Thus, the overall time complexity is \( O(n times 2^n) \).

### Space Complexity

- The space complexity is \( O(2^n) \) to store all subsets in the `ans` vector.

### Code

```cpp
vector<vector<int>> subsets(vector<int>& nums) {
    vector<vector<int>> ans;
    int n = pow(2, nums.size());
    for (int i = 0; i < n; i++) {
        vector<int> temp;
        int x = i;
        int j = 0;
        while (x) {
            if (x & 1) {
                temp.push_back(nums[j]);
            }
            j++;
            x = x >> 1;
        }
        ans.push_back(temp);
    }
    return ans;
}
```

# 124. Print Subsequences

### Problem Link

[Naukri Code360 - Print Subsequences](https://www.naukri.com/code360/problems/print-subsequences_624391)

## Approach: Recursive Backtracking

### Algorithm

1. **Recursive Call**:
   - Make two recursive calls for each character:
     - One including the character in the current subsequence.
     - The other excluding it.
2. **Base Case**: If we reach the end of the input string, add the current subsequence to the results.
3. **Termination**: The recursion terminates once all possible combinations (subsequences) have been generated.

### Time Complexity

- Since each character can either be included or excluded, we have \( 2^n \) subsequences for a string of length \( n \).
- Thus, the time complexity is \( O(2^n) \).

### Space Complexity

- The space complexity is \( O(n) \) for the recursive stack, where \( n \) is the string length.

### Code

```cpp
#include <iostream>
#include <vector>
using namespace std;

void generateSubsequences(char input[], vector<string>& ans, string current, int index) {
    if (input[index] == '\0') {
        ans.push_back(current);
        return;
    }
    generateSubsequences(input, ans, current + input[index], index + 1);
    generateSubsequences(input, ans, current, index + 1);
}

void printSubsequences(char input[]) {
    vector<string> ans;
    generateSubsequences(input, ans, "", 0);
    for (const auto& subseq : ans) {
        cout << subseq << endl;
    }
}
```

# 125. Count Subsets with Sum K

### Problem Link

[Naukri Code360 - Count Subsets with Sum K](https://www.naukri.com/code360/problems/count-subsets-with-sum-k_3952532)

## Approach 1: Recursive Backtracking

### Algorithm

1. **Recursive Call**:
   - For each element, make two recursive calls:
     - One that includes the element in the subset.
     - Another that excludes it from the subset.
2. **Base Case**: If we reach the end of the array and `currentSum` is equal to the target sum \( k \), increase the count.
3. **Termination**: This approach terminates after exploring all subsets.

### Time Complexity

- The time complexity is \( O(2^n) \) because we explore every subset.

### Space Complexity

- The space complexity is \( O(n) \) for the recursive stack.

### Code

```cpp
#include <vector>
using namespace std;

void countSubsets(vector<int>& arr, int index, int currentSum, int targetSum, int& count) {
    if (index == arr.size()) {
        if (currentSum == targetSum) {
            count++;
        }
        return;
    }

    // Include the current element
    countSubsets(arr, index + 1, currentSum + arr[index], targetSum, count);

    // Exclude the current element
    countSubsets(arr, index + 1, currentSum, targetSum, count);
}

int findWays(vector<int>& arr, int k) {
    int count = 0;
    countSubsets(arr, 0, 0, k, count);
    return count;
}
```

## Approach 2: Recursive Backtracking with Sum of Responses

### Algorithm

1. **Recursive Call**:
   - For each element, make two recursive calls:
     - One that includes the element in the subset.
     - Another that excludes it from the subset.
2. **Base Case**: If the end of the array is reached, return `1` if `currentSum` equals the target sum, otherwise return `0`.
3. **Sum of Responses**: At each step, return the sum of the results from including and excluding the current element.

### Time Complexity

- The time complexity is \( O(2^n) \) as it explores every subset.

### Space Complexity

- The space complexity is \( O(n) \) for the recursive stack.

### Code

```cpp
#include <vector>
using namespace std;

int countSubsets(vector<int>& arr, int index, int currentSum, int targetSum) {
    if (index == arr.size()) {
        return (currentSum == targetSum) ? 1 : 0;
    }

    int include = countSubsets(arr, index + 1, currentSum + arr[index], targetSum);
    int exclude = countSubsets(arr, index + 1, currentSum, targetSum);

    return include + exclude;
}

int findWays(vector<int>& arr, int k) {
    return countSubsets(arr, 0, 0, k);
}
```

### Note on Optimization Using Dynamic Programming

The problem of counting subsets with a sum equal to \( k \) can be optimized using dynamic programming. By storing results of subproblems in a DP table, we can avoid redundant calculations and significantly reduce the time complexity from exponential to polynomial. This approach allows us to systematically build up the count of subsets that sum to \( k \) using previously computed results, enhancing efficiency while maintaining clarity in the solution.
