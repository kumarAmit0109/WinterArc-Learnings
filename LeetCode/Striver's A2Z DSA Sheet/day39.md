# 191. Fruit Into Baskets

### Problem Link

[GeeksforGeeks - Fruit Into Baskets](https://www.geeksforgeeks.org/problems/fruit-into-baskets-1663137462/1)

## Approach 1: Brute Force

### Idea/Intuition

This approach uses a nested loop to iterate over all possible subarrays. A set is used to track the types of fruits in the current subarray. If the number of distinct fruits is at most 2, update the maximum length.

### Algorithm

1. **Outer Loop**:
   - Iterate over the array starting from each index.
2. **Inner Loop**:
   - Use a set to track distinct fruit types.
   - Stop when more than 2 types of fruits are found.
   - Update the maximum length if the condition is met.
3. **Return Result**:
   - Return the maximum length of valid subarrays.

### Time Complexity

- **O(n²)**: Nested loops to evaluate all subarrays.

### Space Complexity

- **O(k)**: Space for the set (at most `k` distinct fruits).

### Code

```cpp
int totalFruits(vector<int> &arr) {
    int n = arr.size();
    int maxlen = 0; // To store the maximum length of subarray
    for (int i = 0; i < n; ++i) {
        unordered_set<int> st; // To store the types of fruits in the current window
        for (int j = i; j < n; ++j) {
            st.insert(arr[j]); // Add the current fruit type to the set
            if (st.size() <= 2) {
                // If there are at most 2 types of fruits, calculate the length
                maxlen = max(maxlen, j - i + 1);
            } else {
                // Break if more than 2 types of fruits are in the set
                break;
            }
        }
    }
    return maxlen; // Return the maximum length
}
```

## Approach 2: Sliding Window with Map

### Idea/Intuition

This approach uses a sliding window technique with a hash map to count the frequency of each fruit type in the window. The window is adjusted to maintain at most 2 types of fruits, ensuring a valid subarray.

### Algorithm

1. **Initialize**:
   - Use two pointers (`l` for left, `r` for right) and a hash map to store fruit frequencies.
2. **Expand Window**:
   - Add the current fruit at `r` to the hash map and increase its count.
3. **Shrink Window**:
   - If there are more than 2 types of fruits, move the left pointer (`l`) and update the map until the condition is met.
4. **Update Result**:
   - Track the maximum length of the valid subarray.
5. **Return Result**:
   - Return the maximum length.

### Time Complexity

- **O(n)**: Each fruit is added and removed from the map at most once.

### Space Complexity

- **O(2)**: Space for the map (at most 2 distinct fruits).

### Code

```cpp
int totalFruits(vector<int> &arr) {
    int l = 0, n = arr.size(), maxlen = 0;
    unordered_map<int, int> mpp;

    for (int r = 0; r < n; ++r) {
        mpp[arr[r]]++; // Add the current fruit to the basket

        // If we have more than 2 types of fruits, shrink the window
        while (mpp.size() > 2) {
            mpp[arr[l]]--; // Remove the leftmost fruit
            if (mpp[arr[l]] == 0) {
                mpp.erase(arr[l]); // Remove fruit type from map if its count is 0
            }
            l++; // Move the left pointer
        }

        // Update the maximum length of the valid window
        maxlen = max(maxlen, r - l + 1);
    }

    return maxlen;
}
```

## Approach 3: Optimized Sliding Window with Map

### Idea/Intuition

This optimized approach uses a sliding window and a hash map to maintain the count of fruit types. The window is adjusted dynamically to ensure it always contains at most 2 types of fruits, while keeping track of the maximum valid subarray length.

### Algorithm

1. **Initialize**:
   - Use two pointers (`l` for left, `r` for right) and a hash map to store the count of fruit types in the current window.
2. **Expand Window**:
   - Add the current fruit at index `r` to the hash map and increment its count.
3. **Shrink Window**:
   - When the window contains more than 2 types of fruits, move the left pointer (`l`) and update the map by decrementing counts. Remove the fruit type from the map if its count becomes zero.
4. **Update Result**:
   - Track the maximum length of the valid subarray during each iteration.
5. **Return Result**:
   - Return the maximum valid subarray length.

### Time Complexity

- **O(n)**: Each element is processed at most twice (once when expanding the window and once when shrinking it).

### Space Complexity

- **O(2)**: Space used by the hash map (maximum of 2 distinct fruit types).

### Code

```cpp
int totalFruits(vector<int> &arr) {
    int l = 0, maxlen = 0;
    unordered_map<int, int> mpp;

    for (int r = 0; r < arr.size(); ++r) {
        mpp[arr[r]]++; // Add the current fruit to the basket

        // Shrink the window if it contains more than 2 types of fruits
        while (mpp.size() > 2) {
            mpp[arr[l]]--; // Remove the leftmost fruit
            if (mpp[arr[l]] == 0) {
                mpp.erase(arr[l]); // Remove fruit type if count becomes zero
            }
            l++; // Move the left pointer
        }

        // Update the maximum length of the valid window
        maxlen = max(maxlen, r - l + 1);
    }

    return maxlen;
}
```

# 192. Longest Repeating Character Replacement

### Problem Link

[LeetCode - Longest Repeating Character Replacement](https://leetcode.com/problems/longest-repeating-character-replacement/description/)

## Approach 1: Brute Force Method

### Idea/Intuition

Iterate over all possible substrings, calculate the frequency of characters, and determine the maximum length of a substring that can be converted into the same character by changing at most `k` characters.

### Algorithm

1. Iterate through all substrings starting at index `i`.
2. Use a frequency array to store the counts of characters in the current substring.
3. Update the maximum frequency of any character in the substring.
4. Calculate the number of changes needed to make the substring uniform.
5. If changes exceed `k`, break the loop; otherwise, update the maximum length.

### Time Complexity

- **O(n^2)**: Iterate over all substrings and calculate the frequencies.

### Space Complexity

- **O(26) ≈ O(1)**: Space used by the frequency array.

### Code

```cpp
int characterReplacement(string s, int k) {
    int n = s.size();
    int maxLength = 0;

    for (int i = 0; i < n; ++i) {
        vector<int> hash(26, 0); // Frequency array for characters
        int maxFreq = 0;

        for (int j = i; j < n; ++j) {
            hash[s[j] - 'A']++;
            maxFreq = max(maxFreq, hash[s[j] - 'A']);

            int changes = (j - i + 1) - maxFreq;
            if (changes > k) break;

            maxLength = max(maxLength, j - i + 1);
        }
    }

    return maxLength;
}
```

## Approach 2: Sliding Window

### Idea/Intuition

Use the sliding window technique to maintain a window where the difference between the window size and the maximum frequency of any character is at most `k`. Adjust the window size dynamically to maximize the substring length.

### Algorithm

1. Use two pointers, `left` and `right`, to define the sliding window.
2. Maintain a frequency array to store the counts of characters within the window.
3. Track the maximum frequency of any character in the window.
4. If the number of changes needed to make the substring uniform exceeds `k`, shrink the window from the left.
5. Update the maximum length of the substring during each iteration.

### Time Complexity

- **O(n)**: Each character is processed at most twice (once when expanding the window and once when shrinking).

### Space Complexity

- **O(26) ≈ O(1)**: Space used by the frequency array.

### Code

```cpp
int characterReplacement(string s, int k) {
    int n = s.size();
    int left = 0, right = 0;          // Two pointers for the sliding window
    int maxFreq = 0;                 // Maximum frequency of any character in the current window
    int hash[26] = {0};              // Frequency array for characters
    int maxLength = 0;               // Result to store the maximum length of substring

    while (right < n) {
        hash[s[right] - 'A']++;      // Increment frequency of the current character
        maxFreq = max(maxFreq, hash[s[right] - 'A']); // Update the maximum frequency in the window

        // If the number of changes exceeds k, shrink the window
        while ((right - left + 1) - maxFreq > k) {
            hash[s[left] - 'A']--;   // Decrement frequency of the leftmost character
            left++;                  // Shrink the window from the left
        }

        // Update the maximum length
        maxLength = max(maxLength, right - left + 1);
        right++;
    }

    return maxLength;
}
```

## Approach 3: Optimized Sliding Window

### Idea/Intuition

This is an optimized version of the sliding window approach. Use a single loop to dynamically expand and shrink the window while maintaining the maximum frequency of a character in the current window. This helps in minimizing the number of calculations.

### Algorithm

1. Use two pointers, `i` (left) and `j` (right), to define the sliding window.
2. Maintain a frequency array to store the counts of characters within the window.
3. Track the maximum frequency of any character in the window.
4. Calculate the number of changes needed for the current window. If changes exceed `k`, shrink the window by moving the left pointer `i`.
5. Update the maximum length of the substring during each iteration.

### Time Complexity

- **O(n)**: Each character is processed at most twice (once when expanding the window and once when shrinking).

### Space Complexity

- **O(26) ≈ O(1)**: Space used by the frequency array.

### Code

```cpp
int characterReplacement(string s, int k) {
    int n = s.size();
    vector<int> hash(26, 0); // Array to store frequency of characters
    int maxFreq = 0;         // Stores maximum frequency of a single character
    int maxLength = 0;       // Result to store the maximum length of the substring
    int i = 0;               // Left pointer of the window

    for (int j = 0; j < n; ++j) { // j is the right pointer of the window
        hash[s[j] - 'A']++;       // Increment the frequency of the current character
        maxFreq = max(maxFreq, hash[s[j] - 'A']); // Update max frequency

        // Calculate the number of changes required
        int changes = (j - i + 1) - maxFreq;

        if (changes > k) { // If changes exceed k, shrink the window
            hash[s[i] - 'A']--; // Remove the leftmost character from the window
            i++;                // Move the left pointer
        }

        maxLength = max(maxLength, j - i + 1); // Update the maximum length
    }

    return maxLength;
}
```

# 193. Binary Subarrays with Sum

### Problem Link

[LeetCode - Binary Subarrays with Sum](https://leetcode.com/problems/binary-subarrays-with-sum/submissions/)

## Approach 1: Brute Force

### Idea/Intuition

In this approach, we iterate over all possible subarrays and calculate the sum of each subarray. If the sum equals the `goal`, we increment the count.

### Algorithm

1. **Iterate over all subarrays**:
   - For each subarray, calculate the sum and check if it equals the goal.
2. **Increment count**:
   - If the sum of a subarray equals the `goal`, increment the count.
3. **Return the result**:
   - Return the final count of subarrays that meet the condition.

### Time Complexity

- **O(n^2)**: We iterate over all possible subarrays, and for each subarray, we calculate the sum.

### Space Complexity

- **O(1)**: We use a constant amount of space for variables.

### Code

```cpp
int numSubarraysWithSum(vector<int>& nums, int goal) {
    int count = 0;

    for (int i = 0; i < nums.size(); i++) {
        int sum = nums[i];
        if (sum == goal) {
            count++;
        }
        for (int j = i + 1; j < nums.size(); j++) {
            sum = sum + nums[j];
            if (sum == goal) {
                count++;
            }
            if (sum > goal) {
                break;
            }
        }
    }
    return count;
}
```

## Approach 2: Using Prefix Sum and Hash Map

### Idea/Intuition

This approach uses a hash map to store the frequency of prefix sums. We calculate the current sum and check if the difference between the current sum and the `goal` exists in the map. This allows us to efficiently count the subarrays that satisfy the condition.

### Algorithm

1. **Track prefix sums**:
   - Use a hash map to store the frequency of prefix sums.
2. **Calculate the number of subarrays**:
   - For each number in the array, update the current sum and check if the difference `(currentSum - goal)` exists in the hash map.
3. **Update the map**:
   - Update the hash map with the current sum.

### Time Complexity

- **O(n)**: We process each element in the array a constant number of times.

### Space Complexity

- **O(n)**: We use a hash map to store the prefix sums.

### Code

```cpp
int numSubarraysWithSum(vector<int>& nums, int goal) {
    unordered_map<int, int> prefixSumCount; // To store the frequency of prefix sums
    int currentSum = 0; // To track the current prefix sum
    int count = 0; // To store the total count of subarrays

    // Initialize the hashmap with prefix sum 0
    prefixSumCount[0] = 1;

    for (int num : nums) {
        currentSum += num; // Update the current prefix sum

        // Check if (currentSum - goal) exists in the hashmap
        if (prefixSumCount.find(currentSum - goal) != prefixSumCount.end()) {
            count += prefixSumCount[currentSum - goal];
        }

        // Update the hashmap with the current prefix sum
        prefixSumCount[currentSum]++;
    }

    return count;
}
```

## Approach 3: Using Sliding Window and Helper Function for Subarrays with Sum at Most

### Idea/Intuition

This approach leverages the sliding window technique to count the number of subarrays with a sum less than or equal to a given target. By utilizing the helper function `countSubarraysWithSumAtMost`, we efficiently calculate the number of subarrays with sum equal to the `goal` by subtracting the number of subarrays with sum at most `goal-1` from the number of subarrays with sum at most `goal`.

### Algorithm

1. **Count subarrays with sum at most a target**:
   - Use the sliding window technique to maintain a window of valid subarrays and calculate their sums.
2. **Calculate subarrays with sum exactly equal to the goal**:
   - Subtract the number of subarrays with sum at most `goal-1` from the number of subarrays with sum at most `goal`.

### Time Complexity

- **O(n)**: We process each element in the array once using the sliding window approach.

### Space Complexity

- **O(1)**: We only use a few extra variables for counting.

### Code

```cpp
// Helper function to calculate the count of subarrays with sum <= target
int countSubarraysWithSumAtMost(vector<int>& nums, int target) {
    if (target < 0) return 0; // If target is negative, no valid subarrays
    int left = 0, sum = 0, count = 0;
    for (int right = 0; right < nums.size(); ++right) {
        sum += nums[right]; // Add the current number to the sum

        // Shrink the window from the left until the sum is <= target
        while (sum > target) {
            sum -= nums[left];
            ++left;
        }

        // All subarrays ending at `right` and starting from `left` contribute
        count += (right - left + 1);
    }
    return count;
}

int numSubarraysWithSum(vector<int>& nums, int goal) {
    // Using the property: count(sum == goal) = count(sum <= goal) - count(sum <= goal - 1)
    return countSubarraysWithSumAtMost(nums, goal) - countSubarraysWithSumAtMost(nums, goal - 1);
}
```

# 194. Count Number of Nice Subarrays

### Problem Link

[LeetCode - Count Number of Nice Subarrays](https://leetcode.com/problems/count-number-of-nice-subarrays/description/)

## Approach 1: Brute Force

### Idea/Intuition

- Convert the array into a binary array where odd numbers are represented as `1` and even numbers as `0`.
- Use nested loops to calculate the sum of all possible subarrays and count those that have a sum equal to `k`.

### Algorithm

1. **Convert numbers**:
   - Replace odd numbers with `1` and even numbers with `0`.
2. **Iterate over all subarrays**:
   - For each subarray, compute the sum.
   - Increment the count if the sum equals `k`.
3. **Return the result**:
   - Return the count of valid subarrays.

### Time Complexity

- **O(n²)**: Iterate over all possible subarrays.

### Space Complexity

- **O(1)**: No additional space is required.

### Code

```cpp
int numberOfSubarrays(vector<int>& nums, int k) {
    int n = nums.size(), count = 0;
    // Convert odd numbers to 1 and even numbers to 0
    for (int& num : nums) {
        num = (num % 2 == 1) ? 1 : 0;
    }

    // Brute force: check all subarrays
    for (int i = 0; i < n; ++i) {
        int sum = 0;
        for (int j = i; j < n; ++j) {
            sum += nums[j];
            if (sum == k) {
                count++;
            }
        }
    }
    return count;
}
```

## Approach 2: Sliding Window with atMostK Helper Function

### Idea/Intuition

- Calculate the number of subarrays with at most `k` odd numbers and subtract the number of subarrays with at most `k - 1` odd numbers.
- The difference gives the count of subarrays with exactly `k` odd numbers.

### Algorithm

1. **Helper Function `atMostK`**:

   - Use a sliding window to count subarrays with at most `k` odd numbers.
   - Expand the window by incrementing `right`, and decrement `k` if the current number is odd.
   - Shrink the window from the left until the condition is met.

2. **Calculate Result**:
   - Use the helper function to calculate `atMost(k)` and `atMost(k - 1)`.
   - Subtract `atMost(k - 1)` from `atMost(k)` to get the count of subarrays with exactly `k` odd numbers.

### Time Complexity

- **O(n)**: Each element is processed at most twice (once when expanding and once when contracting the window).

### Space Complexity

- **O(1)**: Constant extra space is used.

### Code

```cpp
int atMostK(vector<int>& nums, int k) {
    int n = nums.size(), left = 0, count = 0;

    for (int right = 0; right < n; ++right) {
        k -= (nums[right] % 2 == 1); // Decrease k if the current number is odd

        while (k < 0) { // Shrink window if there are more than k odd numbers
            k += (nums[left] % 2 == 1);
            left++;
        }

        // All subarrays ending at `right` with at most `k` odd numbers
        count += (right - left + 1);
    }

    return count;
}

int numberOfSubarrays(vector<int>& nums, int k) {
    // Nice subarrays = atMost(k) - atMost(k-1)
    return atMostK(nums, k) - atMostK(nums, k - 1);
}
```

# 195. Number of Substrings Containing All Three Characters

### Problem Link

[LeetCode - Number of Substrings Containing All Three Characters](https://leetcode.com/problems/number-of-substrings-containing-all-three-characters/)

## Approach 1: Brute Force

### Idea/Intuition

- Iterate over all possible substrings of the string.
- For each substring, check if it contains all three characters ('a', 'b', 'c').

### Algorithm

1. Use a nested loop to generate all substrings starting from each index.
2. Maintain a hash array to count the occurrences of 'a', 'b', and 'c' in the current substring.
3. If all three characters are present, increment the count.
4. Return the total count.

### Time Complexity

- **O(n^2)**: Generating all substrings requires quadratic time.
- **O(n)** for checking each substring for the presence of 'a', 'b', 'c'.

### Space Complexity

- **O(1)**: Space for the hash array.

### Code

```cpp
int numberOfSubstrings(string s) {
    int n = s.size();
    int count = 0;

    // Iterate over all possible starting points
    for (int i = 0; i < n; ++i) {
        vector<int> hash(3, 0); // Hash array to count 'a', 'b', 'c'

        // Iterate over all possible ending points
        for (int j = i; j < n; ++j) {
            hash[s[j] - 'a']++; // Increment the count for the character

            // Check if all three characters are present
            if (hash[0] > 0 && hash[1] > 0 && hash[2] > 0) {
                count++;
            }
        }
    }

    return count;
}
```

## Approach 2: Optimized Sliding Window

### Idea/Intuition

- Use a sliding window to track the last seen indices of characters 'a', 'b', and 'c'.
- For every position in the string, calculate the number of valid substrings ending at that position that contain all three characters.

### Algorithm

1. Maintain an array `lastSeen` of size 3 to store the last seen indices of 'a', 'b', and 'c'.
2. Traverse the string:
   - Update the index for the current character in `lastSeen`.
   - If all three characters have been seen at least once, calculate the number of valid substrings ending at the current position as `1 + min(lastSeen[0], lastSeen[1], lastSeen[2])`.
3. Accumulate the count of such substrings.
4. Return the total count.

### Time Complexity

- **O(n)**: Single traversal of the string.

### Space Complexity

- **O(1)**: Space for the `lastSeen` array.

### Code

```cpp
int numberOfSubstrings(string s) {
    int n = s.size();
    vector<int> lastSeen(3, -1); // To track the last seen index of 'a', 'b', and 'c'
    int count = 0;

    // Iterate through the string
    for (int i = 0; i < n; ++i) {
        lastSeen[s[i] - 'a'] = i; // Update the last seen index for the current character

        // Check if all characters 'a', 'b', 'c' have been seen at least once
        if (lastSeen[0] != -1 && lastSeen[1] != -1 && lastSeen[2] != -1) {
            // Add the count of valid substrings ending at i
            count += 1 + min({lastSeen[0], lastSeen[1], lastSeen[2]});
        }
    }

    return count;
}
```
