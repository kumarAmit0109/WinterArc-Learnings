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
