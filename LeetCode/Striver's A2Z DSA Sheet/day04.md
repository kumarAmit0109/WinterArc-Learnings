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

# 18. Maximum Score from Subarray Minimums

### Problem Link

[GeeksforGeeks - Maximum Score from Subarray Minimums](https://www.geeksforgeeks.org/problems/max-sum-in-sub-arrays0824/0)

### Approach 1: Brute Force (Find All Subarrays)

1. **Use two loops** to generate all possible subarrays.
2. For each subarray, **find the smallest and second smallest** elements:
   - Traverse the subarray and keep track of the smallest and second smallest values.
   - If the current element is smaller than the smallest value, update the second smallest and the smallest values.
   - Otherwise, if the current element is smaller than the second smallest, update the second smallest value.
3. **Calculate the score** for each subarray by adding the smallest and second smallest values.
4. **Return the maximum score** obtained from all subarrays.

### Time Complexity

- O(n^2), where n is the number of elements in the array, due to generating all subarrays.

### Space Complexity

- O(1), as we only use a few variables for comparisons.

### Code

```cpp
int pairWithMaxSum(vector<int>& arr) {
    int n = arr.size();
    int maxSum = INT_MIN;

    // Generate all subarrays
    for (int i = 0; i < n - 1; i++) {
        for (int j = i + 1; j < n; j++) {
            // Track the smallest and second smallest elements
            int smallest = INT_MAX, secondSmallest = INT_MAX;
            for (int k = i; k <= j; k++) {
                if (arr[k] < smallest) {
                    secondSmallest = smallest;
                    smallest = arr[k];
                } else if (arr[k] < secondSmallest) {
                    secondSmallest = arr[k];
                }
            }
            // Calculate the score for the current subarray
            int score = smallest + secondSmallest;
            maxSum = max(maxSum, score);
        }
    }

    return maxSum;
}
```

### Approach 2: Optimized Linear Traversal

1. **Iterate through the array** and find consecutive elements.
2. For each pair of consecutive elements, **consider them as the smallest and second smallest**:
   - In every subarray of size 2, the smallest and second smallest elements are the two consecutive numbers.
   - Calculate the sum of these two elements and track the maximum sum.
3. **Return the maximum score** from all consecutive pairs in the array.

### Time Complexity

- O(n), where n is the number of elements in the array, as we only traverse the array once.

### Space Complexity

- O(1), as we only use a few variables to store the max sum and process the array.

### Code

```cpp
int pairWithMaxSum(vector<int>& arr) {
    int n = arr.size();
    int maxSum = INT_MIN;

    // Iterate over consecutive pairs of elements
    for (int i = 0; i < n - 1; i++) {
        int sum = arr[i] + arr[i + 1];
        maxSum = max(maxSum, sum); // Track maximum sum of pairs
    }

    return maxSum;
}
```

# 19. Best Time to Buy and Sell Stock

### Problem Link

[Leetcode - Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/description/)

### Approach 1: Brute Force

1. **Nested Loops**: For each element in the array, consider it as the cost price.
2. **Check Selling Prices**: Traverse the rest of the array using a nested loop to check potential selling prices.
3. **Maintain Maximum Profit**: Calculate profit for each pair of buy and sell prices. If the current profit exceeds the previously recorded profit, update the maximum profit.
4. **Return** the maximum profit at the end.

### Time Complexity

- O(n^2), where n is the number of elements in the prices array.

### Space Complexity

- O(1), since no additional space is used.

### Code

```cpp
int maxProfit(vector<int>& prices) {
    int maxProfit = 0;
    int n = prices.size();

    for (int i = 0; i < n; i++) {
        for (int j = i + 1; j < n; j++) {
            int profit = prices[j] - prices[i];  // Calculate profit
            if (profit > maxProfit) {
                maxProfit = profit;  // Update max profit
            }
        }
    }
    return maxProfit;
}
```

### Approach 2: Optimized Linear Traversal

1. **Initialize Variables**:

   - Set `costPrice` to the first price in the array.
   - Initialize `profit` to 0.

2. **Single Pass**:

   - Traverse through the `prices` array starting from the second element (index 1).

3. **Update Cost Price**:

   - If the current price (`prices[i]`) is lower than the `costPrice`, update `costPrice` to `prices[i]`.

4. **Calculate Profit**:

   - For each price, calculate the profit as the difference between the current price and the `costPrice`.
   - If this calculated profit is greater than the recorded `profit`, update the `profit` variable.

5. **Return** the maximum profit at the end of the traversal.

### Time Complexity

- O(n), where n is the number of elements in the `prices` array.

### Space Complexity

- O(1), since we only use a few variables to track costs and profits.

### Code

```cpp
int maxProfit(vector<int>& prices) {
    int costPrice = prices[0];
    int profit = 0;

    for (int i = 1; i < prices.size(); i++) {
        if (prices[i] < costPrice) {
            costPrice = prices[i];  // Update cost price
        }
        if (prices[i] - costPrice > profit) {
            profit = prices[i] - costPrice;  // Update max profit
        }
    }

    return profit;
}
```

# 20. Rearrange Array Elements by Sign

### Problem Link

[Leetcode - Rearrange Array Elements by Sign](https://leetcode.com/problems/rearrange-array-elements-by-sign/)

## Approach 1: Using Separate Arrays

1. **Initialization**:

   - Store the size of the input array `n`.
   - Create two separate arrays: `pos` for positive numbers and `neg` for negative numbers, each of size `n/2`.

2. **Store Positive and Negative Numbers**:

   - Iterate through the input array `nums`:
     - If the element is positive, store it in the `pos` array and increment the positive index `pi`.
     - If the element is negative, store it in the `neg` array and increment the negative index `ni`.

3. **Fill the Answer Array**:

   - Create an answer array `ans` of size `n`.
   - Use a loop to fill the answer array with alternating positive and negative numbers from the `pos` and `neg` arrays.

4. **Return the Answer**:
   - Return the `ans` array.

### Time Complexity

- O(n), where n is the number of elements in the input array.

### Space Complexity

- O(n), for storing the positive and negative numbers in separate arrays.

### Code

```cpp
vector<int> rearrangeArray(vector<int>& nums) {
    int n = nums.size();
    // Step 1: Store +ve and -ve in separate arrays
    vector<int> pos(n / 2);
    vector<int> neg(n / 2);
    int pi = 0, ni = 0;

    for (int i = 0; i < n; i++) {
        if (nums[i] > 0) {
            pos[pi] = nums[i];
            pi++;
        } else {
            neg[ni] = nums[i];
            ni++;
        }
    }

    // Step 2: Now using those arrays fill ans array
    vector<int> ans(n);
    for (int i = 0; i < n / 2; i++) {
        ans[2 * i] = pos[i];
        ans[2 * i + 1] = neg[i];
    }

    return ans;
}
```

## Approach 2: Without Using Separate Arrays

### Idea Behind the Approach

This approach eliminates the need for extra space to store positive and negative numbers separately. Instead, it directly places the positive and negative elements into the answer array by maintaining two separate indices for positive and negative placements. This ensures that the final arrangement adheres to the required alternating pattern while also improving space efficiency.

### Steps

1. **Initialization**:

   - Create an answer array `ans` of the same size as the input array `nums`.
   - Initialize two indices: `pi` for positive numbers starting at index 0, and `ni` for negative numbers starting at index 1.

2. **Fill the Answer Array**:

   - Iterate through the input array `nums`:
     - If the element is positive, place it at the `pi` index in the `ans` array and increment `pi` by 2.
     - If the element is negative, place it at the `ni` index in the `ans` array and increment `ni` by 2.

3. **Return the Answer**:
   - Return the `ans` array.

### Time Complexity

- O(n), where n is the number of elements in the input array.

### Space Complexity

- O(1), as no additional space is used apart from the answer array.

### Code

```cpp
vector<int> rearrangeArray(vector<int>& nums) {
    vector<int> ans(nums.size());
    int pi = 0, ni = 1;

    for (int i = 0; i < nums.size(); i++) {
        if (nums[i] > 0) {
            ans[pi] = nums[i];
            pi += 2;
        } else {
            ans[ni] = nums[i];
            ni += 2;
        }
    }

    return ans;
}
```
