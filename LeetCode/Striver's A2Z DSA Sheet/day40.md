# 196. Maximum Points You Can Obtain from Cards

### Problem Link

[LeetCode - Maximum Points You Can Obtain from Cards](https://leetcode.com/problems/maximum-points-you-can-obtain-from-cards)

## Approach 1: Sliding Window with Left and Right Sums

### Idea/Intuition

We calculate the maximum points by considering taking cards from both ends of the array. We initially compute the sum of the first `k` cards. Then, we slide the window by removing cards from the left and adding cards from the right, keeping track of the maximum sum.

### Algorithm

1. **Initialize**:
   - Compute the sum of the first `k` cards as `lsum`.
   - Initialize `rsum` to 0 and `maxSum` to `lsum`.
2. **Slide Window**:
   - For each card removed from the left, add one from the right.
   - Update `lsum` and `rsum` accordingly.
   - Track the maximum sum using `maxSum`.
3. **Return Result**:
   - The maximum sum of points collected.

### Time Complexity

- **O(k)**: We process up to `k` elements to calculate the sum.

### Space Complexity

- **O(1)**: Constant space used.

### Code

```cpp
int maxScore(vector<int>& cardPoints, int k) {
    int lsum = 0, rsum = 0, maxSum = 0;
    int n = cardPoints.size();

    // Calculate the initial sum of the first `k` cards
    for (int i = 0; i < k; i++) {
        lsum += cardPoints[i];
    }
    maxSum = lsum;

    int rIndex = n - 1;

    // Slide the window: Remove cards from the left and add cards from the right
    for (int i = k - 1; i >= 0; i--) {
        lsum -= cardPoints[i];
        rsum += cardPoints[rIndex];
        rIndex--;
        maxSum = max(maxSum, lsum + rsum);
    }

    return maxSum;
}
```

## Approach 2: Minimize Subarray Sum of Size `n - k`

### Idea/Intuition

To maximize the points obtained by selecting `k` cards from both ends of the array, we can instead minimize the sum of the subarray of size `n - k` (cards that are not taken). The maximum score is the total sum of the array minus the minimum sum of this subarray.

### Algorithm

1. **Calculate Total Sum**:
   - Compute the sum of all elements in the array as `totalSum`.
2. **Handle Edge Case**:
   - If `k == n`, the result is `totalSum` because all cards are taken.
3. **Find Minimum Subarray Sum**:
   - Use a sliding window approach to find the minimum sum of a subarray of size `n - k`.
4. **Return Result**:
   - The result is `totalSum - minWindowSum`.

### Time Complexity

- **O(n)**: Single traversal to calculate the total sum and find the minimum subarray sum.

### Space Complexity

- **O(1)**: Constant space used.

### Code

```cpp
int maxScore(vector<int>& cardPoints, int k) {
    int n = cardPoints.size();
    int totalSum = 0;

    // Calculate the total sum of the array
    for (int i = 0; i < n; ++i) {
        totalSum += cardPoints[i];
    }

    // If k is equal to the length of the array, just take all the cards
    if (k == n) {
        return totalSum;
    }

    // Find the minimum sum of the subarray of size `n - k`
    int windowSize = n - k;
    int windowSum = 0, minWindowSum = INT_MAX;

    // Initialize the first window
    for (int i = 0; i < windowSize; ++i) {
        windowSum += cardPoints[i];
    }
    minWindowSum = windowSum;

    // Slide the window across the array
    for (int i = windowSize; i < n; ++i) {
        windowSum += cardPoints[i] - cardPoints[i - windowSize];
        minWindowSum = min(minWindowSum, windowSum);
    }

    // Maximum score is the total sum minus the minimum sum of the subarray
    return totalSum - minWindowSum;
}
```

# 197. K Distinct Characters

### Problem Link

[Naukri Code360 - Distinct Characters](https://www.naukri.com/code360/problems/distinct-characters_2221410?)

## Approach 1: Brute Force with Nested Loops

### Idea/Intuition

The idea is to explore all possible substrings and calculate their distinct character count. If the distinct character count is less than or equal to `k`, update the maximum length of the substring.

### Algorithm

1. **Iterate Through All Substrings**:
   - Use two nested loops:
     - Outer loop `i` fixes the starting point of the substring.
     - Inner loop `j` expands the ending point of the substring.
2. **Track Distinct Characters**:
   - Use a hash map to count the frequency of characters in the current substring.
3. **Validate and Update**:
   - If the number of distinct characters is less than or equal to `k`, calculate the length of the substring and update the maximum length.
   - Break the inner loop if the distinct character count exceeds `k`.

### Time Complexity

- **Outer Loop**: \(O(n)\)
- **Inner Loop**: \(O(n)\) (in the worst case for each starting point)
- **Hash Map Operations**: \(O(1)\) per character addition.

Overall: \(O(n^2)\).

### Space Complexity

- **Hash Map**: \(O(k)\) for storing at most `k` distinct characters.

### Implementation

```cpp
int kDistinctChars(int k, string &str) {
    int n = str.size();
    int maxLen = 0;

    // Outer loop to fix the starting point of the substring
    for (int i = 0; i < n; i++) {
        unordered_map<char, int> freqMap; // Map to store character frequencies

        // Inner loop to fix the ending point of the substring
        for (int j = i; j < n; j++) {
            freqMap[str[j]]++; // Add the current character to the map

            // If the number of distinct characters is <= K, update maxLen
            if (freqMap.size() <= k) {
                maxLen = max(maxLen, j - i + 1);
            } else {
                break; // Stop further checks if the distinct character count exceeds K
            }
        }
    }

    return maxLen;
}
```

## Approach 2: Sliding Window with Hash Map

### Idea/Intuition

The idea is to use a sliding window to dynamically track the substring containing at most `k` distinct characters while iterating over the string.

### Algorithm

1. **Initialize Variables**:

   - Use two pointers `start` and `end` to define the sliding window.
   - Use a hash map to track character frequencies in the current window.

2. **Expand the Window**:

   - Incrementally add characters to the window by increasing the `end` pointer.

3. **Shrink the Window**:

   - If the count of distinct characters exceeds `k`, increment the `start` pointer to shrink the window while maintaining at most `k` distinct characters.
   - Remove characters from the hash map if their frequency drops to zero.

4. **Update the Result**:
   - After each expansion, calculate the length of the current valid window and update the maximum length.

### Time Complexity

- **Sliding Window**: \(O(n)\) because each character is added and removed from the hash map at most once.
- **Hash Map Operations**: \(O(1)\) per character.

Overall: \(O(n)\).

### Space Complexity

- **Hash Map**: \(O(k)\) for storing at most `k` distinct characters.

### Implementation

```cpp
int kDistinctChars(int k, string &str) {
    int n = str.size();
    int start = 0, end = 0, maxLength = 0;
    unordered_map<char, int> charCount;

    while (end < n) {
        // Add the current character to the map
        charCount[str[end]]++;

        // If the number of distinct characters exceeds 'k', shrink the window
        while (charCount.size() > k) {
            charCount[str[start]]--;
            if (charCount[str[start]] == 0) {
                charCount.erase(str[start]);
            }
            start++;
        }

        // Update the maximum length of the substring
        maxLength = max(maxLength, end - start + 1);

        // Expand the window
        end++;
    }

    return maxLength;
}
```

## Approach 3: Optimized Sliding Window with Hash Map

### Idea/Intuition

Similar to Approach 2, we use a sliding window and a hash map to track character frequencies. The difference lies in the condition for shrinking the window, which is simplified for optimization.

### Algorithm

1. **Initialize Variables**:

   - Use two pointers `start` and `end` to define the sliding window.
   - Use a hash map to track character frequencies within the current window.

2. **Expand the Window**:

   - Incrementally add characters to the window by increasing the `end` pointer.

3. **Shrink the Window**:

   - When the count of distinct characters exceeds `k`, increment the `start` pointer to shrink the window.
   - Remove characters from the hash map if their frequency becomes zero.

4. **Update the Result**:
   - After each iteration, calculate the length of the current valid window and update the maximum length.

### Time Complexity

- **Sliding Window**: \(O(n)\), as each character is processed once in the expansion and once in the shrinking of the window.
- **Hash Map Operations**: \(O(1)\) per character.

Overall: \(O(n)\).

### Space Complexity

- **Hash Map**: \(O(k)\) for storing at most `k` distinct characters.

### Implementation

```cpp
int kDistinctChars(int k, string &str) {
    int n = str.size();
    int start = 0, end = 0, maxLength = 0;
    unordered_map<char, int> charCount;

    while (end < n) {
        // Add the current character to the map
        charCount[str[end]]++;

        // Shrink the window when distinct characters exceed `k`
        if (charCount.size() > k) {
            charCount[str[start]]--;
            if (charCount[str[start]] == 0) {
                charCount.erase(str[start]);
            }
            start++;
        }

        // Update the maximum length of the substring
        maxLength = max(maxLength, end - start + 1);

        // Expand the window
        end++;
    }

    return maxLength;
}
```

# 198. Subarrays with K Different Integers

### Problem Link

[LeetCode - Subarrays with K Different Integers](https://leetcode.com/problems/subarrays-with-k-different-integers/)

## Approach 1: Brute Force (Iterate All Subarrays)

### Idea/Intuition

We will iterate through all possible subarrays and use a hash map to keep track of the distinct elements. If the number of distinct elements exceeds `k`, we break the inner loop. If the number of distinct elements is exactly `k`, we increment the count.

### Algorithm

1. Iterate through each possible starting index `i` of the subarray.
2. For each starting index, expand the subarray by incrementing the ending index `j`.
3. Maintain a hash map to store the frequency of elements within the subarray.
4. If the size of the hash map exceeds `k`, break the loop.
5. If the size of the hash map is exactly `k`, increment the count.
6. Return the final count.

### Time Complexity

- **Outer Loop**: \(O(n)\)
- **Inner Loop**: In the worst case, the inner loop iterates through the entire array, making it \(O(n)\) in total.

Overall: \(O(n^2)\).

### Space Complexity

- **Hash Map**: \(O(k)\), since we can have at most `k` distinct elements in the current subarray.

### Implementation

```cpp
int subarraysWithKDistinct(vector<int>& nums, int k) {
    int n = nums.size();
    int count = 0;

    // Iterate through all subarrays
    for (int i = 0; i < n; i++) {
        unordered_map<int, int> mpp;

        // Expand the subarray from index i to j
        for (int j = i; j < n; j++) {
            mpp[nums[j]]++;

            // If the number of distinct elements exceeds k, break the loop
            if (mpp.size() > k) {
                break;
            }

            // If the number of distinct elements is exactly k, increment count
            if (mpp.size() == k) {
                count++;
            }
        }
    }

    return count;
}
```

## Approach 2: Sliding Window with Helper Function

### Idea/Intuition

Instead of iterating through all subarrays, we can count the number of subarrays with at most `k` distinct integers using a sliding window technique. The difference between the count of subarrays with at most `k` and at most `k-1` distinct integers will give the number of subarrays with exactly `k` distinct integers.

### Algorithm

1. Define a helper function `countAtMostK` that counts subarrays with at most `k` distinct integers using the sliding window approach.
2. Use the sliding window technique:
   - Expand the window by incrementing the `end` pointer.
   - If the number of distinct integers exceeds `k`, shrink the window by incrementing the `start` pointer.
   - For each valid subarray, count the valid windows.
3. Use the helper function to compute the count of subarrays with exactly `k` distinct integers by subtracting the count of subarrays with at most `k-1` from the count of subarrays with at most `k`.

### Time Complexity

- **Sliding Window**: \(O(n)\) for counting subarrays with at most `k` distinct integers.
- **Final Calculation**: The difference between the counts involves a constant-time operation, so it does not affect the overall complexity.

Overall: \(O(n)\).

### Space Complexity

- **Hash Map**: \(O(k)\), since we are maintaining a hash map to count the frequency of elements within the window.

### Implementation

```cpp
int countAtMostK(vector<int>& nums, int k) {
    int n = nums.size();
    unordered_map<int, int> freq;
    int start = 0, end = 0, count = 0;

    while (end < n) {
        // Expand the window by including nums[end]
        freq[nums[end]]++;

        // Shrink the window if there are more than k distinct integers
        while (freq.size() > k) {
            freq[nums[start]]--;
            if (freq[nums[start]] == 0) {
                freq.erase(nums[start]);
            }
            start++;
        }

        // Count subarrays ending at 'end' with at most k distinct integers
        count += (end - start + 1);
        end++;
    }

    return count;
}

int subarraysWithKDistinct(vector<int>& nums, int k) {
    return countAtMostK(nums, k) - countAtMostK(nums, k - 1);
}
```

# 199. Minimum Window Substring

### Problem Link

[LeetCode - Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring/)

## Approach 1: Brute Force with Substring Validation

### Idea/Intuition

This approach involves generating all possible substrings of `s` and checking each one to determine if it contains all characters from `t` with the required frequencies. We keep track of the smallest valid substring.

### Algorithm

1. Build a frequency map for characters in `t`.
2. Iterate over all possible substrings of `s` using two nested loops:
   - **Outer Loop**: Fix the start of the substring.
   - **Inner Loop**: Expand the substring by moving the end pointer.
3. For each substring, check if it contains all the characters in `t` with required frequencies:
   - Use a helper function to compare the character counts in the substring and `t`.
4. Update the minimum length and starting position whenever a valid substring is found.
5. Return the smallest substring or an empty string if no such substring exists.

### Time Complexity

- Generating substrings: \(O(n^2)\), where \(n\) is the length of `s`.
- Validating each substring: \(O(n)\), as we count and compare characters in the substring.

Overall: \(O(n^3)\).

### Space Complexity

- **Frequency Map**: \(O(m)\), where \(m\) is the number of unique characters in `t`.

### Implementation

```cpp
string minWindow(string s, string t) {
    int n = s.length();
    int minLen = INT_MAX;
    int minStart = 0;

    // Function to check if `current` contains all characters of `t` with required frequencies
    auto containsAll = [](string& substring, map<char, int>& tFreq) {
        map<char, int> currentFreq;
        for (char c : substring) {
            currentFreq[c]++;
        }
        for (auto& [key, value] : tFreq) {
            if (currentFreq[key] < value) return false;
        }
        return true;
    };

    // Frequency map for `t`
    map<char, int> tFreq;
    for (char c : t) {
        tFreq[c]++;
    }

    // Generate all substrings of `s` and check for validity
    for (int i = 0; i < n; ++i) {
        for (int j = i; j < n; ++j) {
            string substring = s.substr(i, j - i + 1);
            if (containsAll(substring, tFreq)) {
                if (substring.length() < minLen) {
                    minLen = substring.length();
                    minStart = i;
                }
            }
        }
    }

    return minLen == INT_MAX ? "" : s.substr(minStart, minLen);
}
```

## Approach 2: Sliding Window with Character Frequency Map

### Idea/Intuition

To find the smallest window in `s` that contains all the characters of `t`, we use a sliding window approach:

1. Maintain a frequency map for characters in `t`.
2. Expand the window by moving the right pointer `j` and decrease the frequency of characters in the map.
3. Once the window satisfies the condition (contains all characters in `t`), try to shrink the window from the left by moving the left pointer `i` while still maintaining the condition.
4. Update the minimum window size whenever a valid window is found.

### Algorithm

1. Count the frequency of each character in `t`.
2. Use two pointers (`i` and `j`) to define the sliding window.
3. Expand the window by moving `j`:
   - If a character in `s[j]` exists in the map and is still needed, decrement the required count.
4. Shrink the window by moving `i`:
   - If the window still satisfies the condition after shrinking, update the minimum window size.
   - Restore the frequency of characters in the map as the window shrinks.
5. Continue until the entire string `s` is traversed.
6. Return the substring of the minimum window, or an empty string if no such window exists.

### Time Complexity

- **Sliding Window**: \(O(n)\), where \(n\) is the length of `s`.
- **Map Operations**: \(O(1)\) per character due to a fixed number of unique characters (256 ASCII).

Overall: \(O(n)\).

### Space Complexity

- **Map Storage**: \(O(m)\), where \(m\) is the number of unique characters in `t`.

### Implementation

```cpp
string minWindow(string s, string t) {
    int n = s.length();
    map<char, int> mp;

    // Count characters in string t
    for (char &ch : t) {
        mp[ch]++;
    }

    int requiredCount = t.length(); // Total characters needed
    int i = 0, j = 0;               // Two pointers for the sliding window
    int minStart = 0;               // Start index of the minimum window
    int minWindow = INT_MAX;        // Length of the minimum window

    while (j < n) {
        char ch_j = s[j]; // Current character at the end of the window
        if (mp[ch_j] > 0)
            requiredCount--;

        mp[ch_j]--; // Decrease frequency in the map

        // Try to shrink the window while it satisfies the condition
        while (requiredCount == 0) {
            if (minWindow > j - i + 1) {
                minWindow = j - i + 1; // Update minimum window length
                minStart = i;         // Update start index of the window
            }

            char ch_i = s[i]; // Current character at the start of the window
            mp[ch_i]++;       // Restore frequency in the map
            if (mp[ch_i] > 0)
                requiredCount++; // If character is required, increase count

            i++; // Shrink the window
        }

        j++; // Expand the window
    }

    // Return the minimum window substring or an empty string if not found
    return minWindow == INT_MAX ? "" : s.substr(minStart, minWindow);
}
```
