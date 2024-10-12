# 56. Find the Smallest Divisor Given a Threshold

## Problem Link

[Find the Smallest Divisor Given a Threshold - LeetCode](https://leetcode.com/problems/find-the-smallest-divisor-given-a-threshold/)

## Approach 1: Brute Force

### Idea

- We check every possible divisor from `1` to `max(arr[])`.
- For each divisor, we calculate the sum of ceiling values after dividing every element by the divisor.
- If the total sum is less than or equal to the threshold, we return that divisor as the answer.

### Time Complexity

- **O(n × max(arr[]))**  
  We iterate through all possible divisors, and for each, calculate the sum in O(n) time.

### Space Complexity

- **O(1)**  
  Only a few variables are used.

### Code

```cpp
int Sum(vector<int>& arr, int div) {
    int sum = 0;
    for (int num : arr) {
        sum += ceil((double)num / div);
    }
    return sum;
}

int smallestDivisor(vector<int>& nums, int threshold) {
    int maxVal = *max_element(nums.begin(), nums.end());
    for (int d = 1; d <= maxVal; d++) {
        if (Sum(nums, d) <= threshold) {
            return d;
        }
    }
    return -1;
}
```

## Approach 2: Binary Search

### Idea

- We use binary search to efficiently find the smallest divisor.
- The search space is between `1` and `max(arr[])`.
- For each mid value in the search, we calculate the sum using the `Sum()` function.
- If the sum is within the threshold, we store the mid as the answer and explore the left half for a smaller divisor.
- If the sum exceeds the threshold, we explore the right half.

### Time Complexity

- **O(n × log(max(arr[])))**  
  Binary search runs in O(log(max(arr[]))), and each sum calculation takes O(n).

### Space Complexity

- **O(1)**  
  Only a few extra variables are used.

### Code

```cpp
int Sum(vector<int>& arr, int div) {
    int sum = 0;
    for (int num : arr) {
        sum += ceil((double)num / div);
    }
    return sum;
}

int findMax(vector<int>& arr) {
    int maxi = INT_MIN;
    for (int num : arr) {
        maxi = max(maxi, num);
    }
    return maxi;
}

int smallestDivisor(vector<int>& nums, int threshold) {
    int low = 1, high = findMax(nums), ans = -1;

    while (low <= high) {
        int mid = low + (high - low) / 2;
        if (Sum(nums, mid) <= threshold) {
            ans = mid;
            high = mid - 1;
        } else {
            low = mid + 1;
        }
    }
    return ans;
}
```

# 57. Capacity to Ship Packages Within D Days

## Problem Link

[Capacity to Ship Packages Within D Days - LeetCode](https://leetcode.com/problems/capacity-to-ship-packages-within-d-days/description/)

## Approach 1: Brute-force

### Idea

- Check all possible capacities to find the minimum one that allows shipping within the given days.

### Steps:

1. Loop through each capacity (denote it as `cap`).
2. For each `cap`, call the `findDays()` function to determine the number of days required to ship the packages.
3. Return the minimum `cap` for which the number of days is less than or equal to `d`.

### Time Complexity

- **O(n \* m)**  
  Where `n` is the number of packages and `m` is the maximum capacity.

### Space Complexity

- **O(1)**  
  Only a few extra variables are used.

### Code

```cpp
int findDays(vector<int>& weights, int cap) {
    int days = 1;
    int load = 0;
    for (int weight : weights) {
        if (load + weight > cap) {
            days += 1;
            load = weight;
        } else {
            load += weight;
        }
    }
    return days;
}

int shipWithinDays(vector<int>& weights, int days) {
    int ans = INT_MAX;
    for (int cap = 1; cap <= accumulate(weights.begin(), weights.end(), 0); ++cap) {
        if (findDays(weights, cap) <= days) {
            ans = min(ans, cap);
        }
    }
    return ans;
}
```

## Approach 2: Binary Search

### Idea

- Use binary search to find the minimum capacity that allows shipping within the given days.

### Steps:

1. Create a function `ranges()` to find the minimum and maximum possible capacities.
2. Use binary search to narrow down the possible capacities:
   - Initialize `strt` to the minimum capacity and `end` to the maximum capacity.
   - Calculate the `mid` capacity and find the number of days required using `findDays()`.
   - If the required days are within the limit, update the answer and search for smaller capacities; otherwise, search for larger capacities.

### Time Complexity

- **O(n \* log(max(weights)))**  
  Where `n` is the number of packages and `log(max(weights))` is from the binary search.

### Space Complexity

- **O(1)**  
  Only a few extra variables are used.

### Code

```cpp
pair<int,int> ranges(vector<int>& arr) {
    pair<int,int> p;
    int mini = INT_MIN; // to find max weight
    int maxi = 0;
    for (int weight : arr) {
        mini = max(mini, weight);
        maxi += weight;
    }
    p.first = mini;
    p.second = maxi;
    return p;
}

int findDays(vector<int> &weights, int cap) {
    int days = 1;
    int load = 0;
    int n = weights.size();
    for (int i = 0; i < n; i++) {
        if (load + weights[i] > cap) {
            days += 1;
            load = weights[i];
        } else {
            load += weights[i];
        }
    }
    return days;
}

int shipWithinDays(vector<int>& weights, int days) {
    pair<int,int> range = ranges(weights);
    int strt = range.first, end = range.second;
    int ans = 0;

    while (strt <= end) {
        int mid = strt + (end - strt) / 2;
        int calDays = findDays(weights, mid);

        if (calDays <= days) {
            ans = mid;
            end = mid - 1;
        } else {
            strt = mid + 1;
        }
    }
    return strt;
}
```

# 57. Kth Missing Positive Number

### Problem Link

[Kth Missing Positive Number - LeetCode](https://leetcode.com/problems/kth-missing-positive-number/)

## Approach 1: Linear Scan

### Idea

Traverse the array and increment the value of `k` for each positive integer in the array that is less than or equal to `k`.

### Steps:

1. Iterate through the array. For each element `arr[i]`, check if it is less than or equal to `k`.
2. If the condition holds, increment `k` by 1.
3. If you encounter an element greater than `k`, break out of the loop.
4. Finally, return the updated value of `k`, which will be the `k`th missing positive integer.

### Time Complexity

- **O(n)** as we may need to traverse the entire array.

### Space Complexity

- **O(1)** since no extra space is used.

### Code

```cpp
int findKthPositive(vector<int>& arr, int k) {
    for (int i = 0; i < arr.size(); i++) {
        if (arr[i] <= k) {
            k++; // Shifting k
        } else {
            break;
        }
    }
    return k; // Return the k-th missing positive integer
}
```

## Approach 2: Binary Search

### Idea

Use binary search to efficiently find the `k`th missing positive number by leveraging the relationship between the index and the value of the elements in the array.

### Steps:

1. **Initialization:**

   - Set two pointers, `strt` to `0` and `end` to the last index of the array (`arr.size() - 1`).

2. **Binary Search:**

   - While `strt` is less than or equal to `end`:
     - Calculate `mid = strt + (end - strt) / 2`.
     - Calculate the number of missing positive integers up to `arr[mid]` using the formula: `missing = arr[mid] - (mid + 1)`.
     - If `missing < k`, move the search to the right half by setting `strt = mid + 1`.
     - Otherwise, move to the left half by setting `end = mid - 1`.

3. **Result Calculation:**
   - The `k`th missing positive integer can be calculated as `ans = strt + k`.

### Time Complexity

- **O(log n)** due to the binary search process.

### Space Complexity

- **O(1)** as no extra space is used.

### Code

```cpp
int findKthPositive(vector<int>& arr, int k) {
    int strt = 0, end = arr.size() - 1;
    while (strt <= end) {
        int mid = strt + (end - strt) / 2;
        int missing = arr[mid] - (mid + 1);
        if (missing < k) {
            strt = mid + 1;
        } else {
            end = mid - 1;
        }
    }
    int ans = strt + k;
    return ans;
}
```
