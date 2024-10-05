# 16. Majority Element

### Problem Link

[Leetcode - Majority Element](https://leetcode.com/problems/majority-element/description/)

---

### Approach 1: Brute Force

1. **Iterate** through each element of the array.
2. **Count** how many times each element appears in the entire array.
3. **Check** if any element's count is greater than `n/2`, where `n` is the size of the array.
4. **Return** the majority element if found.

### Time Complexity

- O(n^2), where n is the number of elements in the array.

### Space Complexity

- O(1), as no additional space is used except for a few variables.

### Code

```cpp
int majorityElement(vector<int>& nums) {
    int n = nums.size();

    // Brute force approach: Check every element
    for (int i = 0; i < n; i++) {
        int count = 0;

        // Count the occurrences of nums[i]
        for (int j = 0; j < n; j++) {
            if (nums[i] == nums[j]) {
                count++;
            }
        }

        // If count is greater than n/2, return nums[i]
        if (count > n / 2) {
            return nums[i];
        }
    }

    return -1; // Default case, though not expected
}
```

## Approach 2: Hash Map (Frequency Count)

1. **Initialize** a hash map to store the frequency count of each element.
2. **Iterate** through the array to populate the frequency map.
3. **Traverse** the map and return the element whose frequency is greater than `n/2`, where `n` is the size of the array.

### Time Complexity

- O(n), where `n` is the number of elements in the array.

### Space Complexity

- O(n), for storing the frequency counts in the hash map.

### Code

```cpp
int majorityElement(vector<int>& nums) {
    unordered_map<int, int> freqMap;
    int n = nums.size();

    // Step 1: Count the frequency of each element
    for (int num : nums) {
        freqMap[num]++;
    }

    // Step 2: Return the element with a frequency greater than n/2
    for (auto& entry : freqMap) {
        if (entry.second > n / 2) {
            return entry.first;
        }
    }

    return -1; // Default case, if no majority element is found
}
```

### Approach 3: Sorting

1. **Sort** the array.
2. **Return** the element at the index `n/2`, where `n` is the size of the array. The majority element is guaranteed to be at this position in a sorted array.

### Time Complexity

- **O(n log n)**, due to sorting the array.

### Space Complexity

- **O(1)**, if sorting is done in-place, or **O(n)** if additional space is required for sorting.

### Code

```cpp
int majorityElement(vector<int>& nums) {
    // Sort the array
    sort(nums.begin(), nums.end());

    // Return the element at index n/2
    return nums[nums.size() / 2];
}
```

### Approach 4: Moore's Voting Algorithm

1. **Initialize** two variables: `candidate` to store the potential majority element, and `count` to store its frequency.
2. **Traverse** through the array:
   - If `count` is zero, set the current element as the `candidate`.
   - Increment or decrement `count` based on whether the current element is the `candidate`.
3. **Return** the `candidate` as the majority element.

### Time Complexity

- **O(n)**, where n is the number of elements in the array.

### Space Complexity

- **O(1)**, since we only use two variables.

### Code

```cpp
int majorityElement(vector<int>& nums) {
    int candidate = 0, count = 0;

    // Moore's Voting Algorithm
    for (int num : nums) {
        if (count == 0) {
            candidate = num;
        }
        count += (num == candidate) ? 1 : -1;
    }

    return candidate;
}
```

# 17. Maximum Subarray

### Problem Link

[Leetcode - Maximum Subarray](https://leetcode.com/problems/maximum-subarray/)

---

### Approach 1: Brute Force (Generate All Subarrays)

1. **Generate all possible subarrays** by using two nested loops.
2. **Calculate the sum** of each subarray.
3. **Track** the maximum sum encountered and return it at the end.

### Time Complexity

- **O(n^2)**, where `n` is the number of elements in the array, as we generate and sum all possible subarrays.

### Space Complexity

- **O(1)**, since no additional space is used except for variables.

### Code

```cpp
int maxSubArray(vector<int>& nums) {
    int n = nums.size();
    int maxSum = INT_MIN;

    // Brute force approach: Generate all subarrays
    for (int i = 0; i < n; i++) {
        int currentSum = 0;
        for (int j = i; j < n; j++) {
            currentSum += nums[j];
            maxSum = max(maxSum, currentSum);
        }
    }

    return maxSum;
}
```

### Approach 2: Kadane's Algorithm

1. **Initialize** two variables:
   - `maxi` (maximum sum) to store the highest sum encountered.
   - `sum` (current sum) to track the ongoing subarray sum.
2. **Traverse** the array:
   - Add each element to `sum`.
   - If `sum` exceeds `maxi`, update `maxi`.
   - If `sum` becomes negative, reset `sum` to 0, as a negative sum will reduce the overall sum of subsequent subarrays.
3. **Return** `maxi`, which holds the maximum subarray sum.

### Time Complexity

- **O(n)**, where `n` is the number of elements in the array, since we traverse the array once.

### Space Complexity

- **O(1)**, as only a few variables are used.

### Code

```cpp
int maxSubArray(vector<int>& nums) {
    // Kadane's Algorithm
    int maxi = INT_MIN;
    int sum = 0;

    // Traverse through the array
    for (int i = 0; i < nums.size(); i++) {
        sum += nums[i];

        // Update maxi if the current sum is greater than maxi
        if (sum > maxi) {
            maxi = sum;
        }

        // If the sum becomes negative, reset it to 0
        if (sum < 0) {
            sum = 0;
        }
    }

    return maxi;
}
```
