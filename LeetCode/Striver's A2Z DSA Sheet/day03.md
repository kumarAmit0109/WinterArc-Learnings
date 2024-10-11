# 11. Single Number

## Problem Link

[Single Number - LeetCode](https://leetcode.com/problems/single-number/description/)

---

## Approach: Using XOR

### Idea

We can leverage the properties of XOR to solve this problem efficiently:

1. **Property of XOR**: `a ^ a = 0` and `a ^ 0 = a`.  
   This means when a number is XORed with itself, the result is 0.  
   When a number is XORed with 0, the result is the number itself.
2. By XORing all the numbers in the array, the duplicates cancel each other out, and the result will be the single number.

### Time Complexity

- **Time Complexity**: `O(n)`  
  We traverse the array once to compute the result, where `n` is the number of elements in the array.

### Space Complexity

- **Space Complexity**: `O(1)`  
  No extra space is used apart from a few variables.

---

## Code

```cpp
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        int ans = 0; // Initialize the result variable

        // Traverse the array and XOR each element with the result
        for (int i = 0; i < nums.size(); i++) {
            ans = ans ^ nums[i];
        }

        // The result will be the single number
        return ans;
    }
};
```

# 12. Longest Subarray with Sum K

### Problem Link

[Naukri Code360 - Longest Subarray with Sum K](https://www.naukri.com/code360/problems/longest-subarray-with-sum-k_6682399)

---

## Approach 1: Brute Force

### Idea

In this approach, we generate all possible subarrays and calculate their sum. If the sum of the subarray is equal to `K`, we update the maximum length of the subarray.

### Time Complexity

**Time Complexity**: O(n²)  
We generate all possible subarrays using two loops and calculate their sum, leading to quadratic time complexity.

### Space Complexity

**Space Complexity**: O(1)  
We use only a few extra variables, so the space complexity is constant.

### Code

```cpp
int longestSubarrayWithSumK(vector<int>& a, long long k) {
    int n = a.size();
    int maxLength = 0;

    // Brute force: Generate all subarrays
    for (int i = 0; i < n; i++) {
        long long currentSum = 0;

        for (int j = i; j < n; j++) {
            currentSum += a[j];

            // If subarray sum equals K, update maxLength
            if (currentSum == k) {
                maxLength = max(maxLength, j - i + 1);
            }
        }
    }

    return maxLength;
}
```

## Approach 2: Prefix Sum with Hashmap

### Idea

This approach uses a hashmap (or `unordered_map`) to store the prefix sums and their corresponding indices. The prefix sum is computed while iterating through the array, and the difference between the current prefix sum and `K` is checked. If the difference exists in the hashmap, we compute the subarray length and update the maximum length.

### Steps:

1. Declare a map to store prefix sums and indices.
2. Iterate through the array and calculate the prefix sum.
3. If the prefix sum is equal to `K`, update the max length.
4. Check if the prefix sum minus `K` exists in the map, then calculate the length of the subarray.
5. Update the map with the current prefix sum and index.

### Time Complexity

**Time Complexity**: O(n)  
We traverse the array once, and each lookup in the hashmap takes constant time.

### Space Complexity

**Space Complexity**: O(n)  
We use extra space for the hashmap to store the prefix sums.

### Code

```cpp
int longestSubarrayWithSumK(vector<int>& a, long long k) {
    unordered_map<long long, int> prefixMap;
    long long sum = 0;
    int maxLength = 0;

    for (int i = 0; i < a.size(); i++) {
        sum += a[i];

        // Check if subarray from the start has sum K
        if (sum == k) {
            maxLength = i + 1;
        }

        // Check if sum - k exists in the map
        if (prefixMap.find(sum - k) != prefixMap.end()) {
            maxLength = max(maxLength, i - prefixMap[sum - k]);
        }

        // Store the first occurrence of the prefix sum
        if (prefixMap.find(sum) == prefixMap.end()) {
            prefixMap[sum] = i;
        }
    }

    return maxLength;
}
```

## Approach 3: Two Pointer (Sliding Window)

### Idea

The two-pointer approach keeps track of the current sum using two pointers (left and right). Initially, both pointers are at the start of the array. We adjust the window by moving the right pointer to include elements and the left pointer to exclude elements to maintain the sum equal to `K`.

### Steps:

1. Initialize both `left` and `right` pointers at the start of the array.
2. Add the element pointed by the `right` pointer to the current sum.
3. If the current sum exceeds `K`, move the `left` pointer right to reduce the sum.
4. If the current sum equals `K`, update the maximum subarray length.
5. Continue moving the `right` pointer until the end of the array.

### Time Complexity

**Time Complexity**: O(n)  
Both pointers traverse the array once.

### Space Complexity

**Space Complexity**: O(1)  
No extra data structures are used.

### Code

```cpp
int longestSubarrayWithSumK(vector<int> a, long long k) {
    int left = 0, right = 0;
    long long currentSum = 0;
    int maxLength = 0;

    // Two pointer (sliding window) approach
    while (right < a.size()) {
        currentSum += a[right];

        // Adjust the left pointer if the currentSum exceeds K
        while (currentSum > k && left <= right) {
            currentSum -= a[left];
            left++;
        }

        // If currentSum equals K, update maxLength
        if (currentSum == k) {
            maxLength = max(maxLength, right - left + 1);
        }

        right++;
    }

    return maxLength;
}
```

# 13. Longest Subarray with Sum K | [Postives and Negatives]

### Problem Link

[GeeksforGeeks - Longest Subarray with Sum K](https://www.geeksforgeeks.org/problems/longest-sub-array-with-sum-k0809/1)

## Approach 1: Brute Force

### Idea

In this approach, we generate all possible subarrays and calculate their sum. If the sum of the subarray is equal to K, we update the maximum length of the subarray.

### Time Complexity

O(n²): We generate all possible subarrays using two loops and calculate their sum, leading to quadratic time complexity.

### Space Complexity

O(1): We use only a few extra variables, so the space complexity is constant.

### Code

```cpp
int lenOfLongSubarr(int A[],  int N, int K) {
    int maxLength = 0;

    // Brute force: Generate all subarrays
    for (int i = 0; i < N; i++) {
        int currentSum = 0;

        for (int j = i; j < N; j++) {
            currentSum += A[j];

            // If subarray sum equals K, update maxLength
            if (currentSum == K) {
                maxLength = max(maxLength, j - i + 1);
            }
        }
    }

    return maxLength;
}
```

## Approach 2: Prefix Hashmap

### Idea

We will use a hashmap to store the prefix sums and their indices. For each index, we will check if the current prefix sum minus K exists in the hashmap, which would indicate that there is a subarray that sums to K.

### Time Complexity

O(n): We traverse the array once, performing constant-time operations for each element.

### Space Complexity

O(n): We may store up to n prefix sums in the hashmap.

### Code

```cpp
int lenOfLongSubarr(int A[], int N, int K) {
    unordered_map<int, int> preSumMap; // Map to store prefix sums and their indices
    int maxLength = 0;
    int currentSum = 0;

    for (int i = 0; i < N; i++) {
        currentSum += A[i];

        // Check if the current prefix sum equals K
        if (currentSum == K) {
            maxLength = i + 1; // Update the maxLength for subarray from index 0 to i
        }

        // Check if currentSum - K exists in the map
        if (preSumMap.find(currentSum - K) != preSumMap.end()) {
            maxLength = max(maxLength, i - preSumMap[currentSum - K]);
        }

        // Store the current prefix sum if not already in the map
        if (preSumMap.find(currentSum) == preSumMap.end()) {
            preSumMap[currentSum] = i;
        }
    }

    return maxLength;
}
```

# 14. Two Sum

### Problem Link

[Two Sum](https://leetcode.com/problems/two-sum/description/)

---

### Approach 1: Brute Force

1. **Initialize** the size of the array `n`.
2. **Use nested loops** to check every possible pair of indices:
   - For each index `i`, check all subsequent indices `j` (where `j > i`).
   - If the sum of `nums[i]` and `nums[j]` equals the target, return the indices `{i, j}`.
3. **Return** an empty vector if no solution is found.

### Time Complexity

- O(n^2), where n is the number of elements in the array.

### Space Complexity

- O(1), as no additional space is used.

### Code

```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        int n = nums.size();
        for (int i = 0; i < n - 1; i++) {
            for (int j = i + 1; j < n; j++) {
                if (nums[i] + nums[j] == target) {
                    return {i, j};
                }
            }
        }
        return {}; // No solution found
    }
};
```

### Approach 2: Hash Map for Lookup

1. **Initialize** a hash map to store the numbers and their indices.
2. **Build the hash table** by iterating through the array:
   - Store each number as a key and its index as the value.
3. **Find the complement** for each number:
   - For each index `i`, calculate `complement = target - nums[i]`.
   - Check if the complement exists in the hash map and ensure it's not the same index.
   - If found, return `{i, numMap[complement]}`.
4. **Return** an empty vector if no solution is found.

### Time Complexity

- O(n), where n is the number of elements in the array.

### Space Complexity

- O(n), as we use additional space for the hash map.

### Code

```cpp
vector<int> twoSum(vector<int>& nums, int target) {
    unordered_map<int, int> numMap;
    int n = nums.size();

    // Build the hash table
    for (int i = 0; i < n; i++) {
        numMap[nums[i]] = i;
    }

    // Find the complement
    for (int i = 0; i < n; i++) {
        int complement = target - nums[i];
        if (numMap.count(complement) && numMap[complement] != i) {
            return {i, numMap[complement]};
        }
    }

    return {}; // No solution found
}
```

### Approach 3: Optimized Hash Map

1. **Initialize** a hash map for storing numbers and their indices.
2. **Iterate** through the array:
   - For each element, calculate its complement: `complement = target - nums[i]`.
   - Check if the complement exists in the hash map.
   - If found, return the indices `{numMap[complement], i}`.
   - Otherwise, store the current number and its index in the hash map.
3. **Return** an empty vector if no solution is found.

### Time Complexity

- O(n), where n is the number of elements in the array.

### Space Complexity

- O(n), as we use additional space for the hash map.

### Code

```cpp
vector<int> twoSum(vector<int>& nums, int target) {
    unordered_map<int, int> numMap;
    int n = nums.size();

    for (int i = 0; i < n; i++) {
        int complement = target - nums[i];
        if (numMap.count(complement)) {
            return {numMap[complement], i};
        }
        numMap[nums[i]] = i;
    }

    return {}; // No solution found
}
```

# 15. Sort Colors

### Problem Link

## [Sort Colors](https://leetcode.com/problems/sort-colors/description/)

### Approach 1: Counting Sort

1. **Initialize** counters for 0s, 1s, and 2s.
2. **Count** the occurrences of each color in the array.
3. **Fill** the array based on these counts:
   - Fill in 0s based on the count of 0s.
   - Fill in 1s based on the count of 1s.
   - Fill in 2s based on the count of 2s.

### Time Complexity

- O(n), where n is the number of elements in the array.

### Space Complexity

- O(1), since only a fixed number of counters are used.

### Code

```cpp
void sortColors(vector<int>& nums) {
    int zeroCount = 0, oneCount = 0, twoCount = 0;

    // Step 1: Count the occurrences of 0s, 1s, and 2s
    for (int i = 0; i < nums.size(); i++) {
        if (nums[i] == 0) {
            zeroCount++;
        } else if (nums[i] == 1) {
            oneCount++;
        } else {
            twoCount++;
        }
    }

    // Step 2: Fill the array based on counts
    int i = 0;
    while (zeroCount > 0) {
        nums[i] = 0;
        i++;
        zeroCount--;
    }
    while (oneCount > 0) {
        nums[i] = 1;
        i++;
        oneCount--;
    }
    while (twoCount > 0) {
        nums[i] = 2;
        i++;
        twoCount--;
    }
}
```

### Approach 2: Dutch National Flag Algorithm

1. **Initialize** three pointers:
   - `low` for the next position of 0
   - `mid` for the current element
   - `high` for the next position of 2
2. **Traverse** through the array using the `mid` pointer:
   - If `nums[mid]` is 0, swap it with `nums[low]`, increment both `low` and `mid`.
   - If `nums[mid]` is 1, simply increment `mid`.
   - If `nums[mid]` is 2, swap it with `nums[high]` and decrement `high`.
3. **Return** the sorted array.

### Time Complexity

- O(n), where n is the number of elements in the array.

### Space Complexity

- O(1), since no additional space is used.

### Code

```cpp
void sortColors(vector<int>& nums) {
    int low = 0, mid = 0, high = nums.size() - 1;
    while (mid <= high) {
        if (nums[mid] == 0) {
            swap(nums[mid], nums[low]);
            low++;
            mid++;
        } else if (nums[mid] == 1) {
            mid++;
        } else {
            swap(nums[mid], nums[high]);
            high--;
        }
    }
}
```
