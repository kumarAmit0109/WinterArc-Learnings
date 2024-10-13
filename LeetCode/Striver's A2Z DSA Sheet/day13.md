# 61. Split Array Largest Sum

### Problem Link

[Split Array Largest Sum - LeetCode](https://leetcode.com/problems/split-array-largest-sum/description/)

## Approach 1: Linear Search on Possible Sums

### Idea

1. **Find boundaries for the search space:**

   - `max(arr[])`: The largest element in the array.
   - `sum(arr[])`: The sum of all elements in the array.

2. **Iterate over possible answers:**

   - Use a loop with `maxSum` ranging from `max(arr[])` to `sum(arr[])`.
   - For each `maxSum`, pass it to `countPartitions()` to find the number of partitions needed.

3. **Check for valid partitions:**

   - If `countPartitions(maxSum) == k`, then return `maxSum` as the answer. This will be the first valid answer with exactly `k` partitions.

4. **Return max(arr[]) if no other answer is found.**

### Time Complexity

- **O(n \* (sum - max))**, where `n` is the size of the array.

### Space Complexity

- **O(1)** as no extra space is used.

### Code

```cpp
int countPartitions(vector<int>& nums, int maxSum) {
    int partitions = 1;
    int currentSum = 0;

    for (int num : nums) {
        if (currentSum + num <= maxSum) {
            currentSum += num;
        } else {
            partitions++;
            currentSum = num;
        }
    }
    return partitions;
}

int splitArray(vector<int>& nums, int k) {
    int maxElement = *max_element(nums.begin(), nums.end());
    int totalSum = accumulate(nums.begin(), nums.end(), 0);

    for (int maxSum = maxElement; maxSum <= totalSum; maxSum++) {
        if (countPartitions(nums, maxSum) == k) {
            return maxSum;
        }
    }
    return maxElement;
}
```

## Approach 2: Binary Search on Sum

### Idea

1. **Set the search space:**

   - `strt = max(arr[])`: The smallest possible sum.
   - `end = sum(arr[])`: The largest possible sum.

2. **Binary Search:**

   - Calculate `mid = (strt + end) / 2`.
   - Use the `calculate()` function to determine how many partitions are needed with a maximum sum of `mid`.

3. **Adjust search space:**

   - If the number of partitions is less than or equal to `k`, try a smaller sum by setting `end = mid - 1`.
   - If more partitions are required, increase the sum by setting `strt = mid + 1`.

4. **Return `strt` as the answer.**

### Time Complexity

- **O(n \* log(sum - max))**, where `n` is the size of the array.

### Space Complexity

- **O(1)** as no extra space is used.

### Code

```cpp
int calculate(vector<int>& nums, int mid) {
    int partitions = 1;
    int currentSum = 0;

    for (int num : nums) {
        if (currentSum + num <= mid) {
            currentSum += num;
        } else {
            partitions++;
            currentSum = num;
        }
    }
    return partitions;
}

int splitArray(vector<int>& nums, int k) {
    int strt = *max_element(nums.begin(), nums.end());
    int end = accumulate(nums.begin(), nums.end(), 0);

    while (strt <= end) {
        int mid = strt + (end - strt) / 2;
        int calSubarray = calculate(nums, mid);

        if (calSubarray <= k) {
            end = mid - 1;
        } else {
            strt = mid + 1;
        }
    }
    return strt;
}
```

# 62. Painter's Partition Problem

### Problem Link

[Painter's Partition Problem - Naukri](https://www.naukri.com/code360/problems/painter-s-partition-problem_1089557?)

## Approach 1: Linear Search on Time

### Idea

1. **Set the search space:**

   - Start from `max(arr[])` (minimum time required to paint the largest board).
   - End at `sum(arr[])` (maximum time if a single painter paints all boards).

2. **Iterate over all possible times:**

   - For each possible time value, check how many painters are required using the `countPainters()` function.

3. **Find the answer:**
   - Return the first time value for which the number of painters required is less than or equal to `k`.
   - If the loop ends, return `max(arr[])` as no valid solution can have a time smaller than this value.

### Time Complexity

- **O(n \* (sum - max))** where `n` is the size of the array.

### Space Complexity

- **O(1)** since no extra space is used.

### Code

```cpp
int countPainters(vector<int> &boards, int time) {
    int total = 0;
    int painters = 1;

    for (int board : boards) {
        if (total + board <= time) {
            total += board;
        } else {
            painters++;
            total = board;
        }
    }
    return painters;
}

int findLargestMinDistance(vector<int> &boards, int k) {
    int maxBoard = *max_element(boards.begin(), boards.end());
    int totalSum = accumulate(boards.begin(), boards.end(), 0);

    for (int time = maxBoard; time <= totalSum; time++) {
        if (countPainters(boards, time) <= k) {
            return time;
        }
    }
    return maxBoard;
}
```

## Approach 2: Binary Search on Time

### Idea

1. **Set the search space:**

   - `strt = max(arr[])`: The smallest possible time.
   - `end = sum(arr[])`: The largest possible time.

2. **Binary Search:**

   - Calculate `mid = (strt + end) / 2`.
   - Use the `calCount()` function to determine how many painters are required with a maximum time of `mid`.

3. **Adjust the search space:**

   - If the number of painters is more than `k`, increase the minimum time by setting `strt = mid + 1`.
   - If the number of painters is less than or equal to `k`, update the result to `mid` and try for a smaller time by setting `end = mid - 1`.

4. **Return the result:**
   - After the binary search completes, return the result.

### Time Complexity

- **O(n \* log(sum - max))**, where `n` is the size of the array.

### Space Complexity

- **O(1)** as no extra space is used.

### Code

```cpp
int calCount(vector<int> &boards, int mid) {
    int total = 0;
    int count = 1;

    for (int i = 0; i < boards.size(); i++) {
        if (total + boards[i] <= mid) {
            total += boards[i];
        } else {
            count++;
            total = boards[i];
        }
    }
    return count;
}

int findLargestMinDistance(vector<int> &boards, int k) {
    int strt = *max_element(boards.begin(), boards.end());
    int end = accumulate(boards.begin(), boards.end(), 0);
    int result = -1; // Initialize result variable

    while (strt <= end) {
        int mid = strt + (end - strt) / 2;
        int count = calCount(boards, mid);

        if (count > k) {
            strt = mid + 1;
        } else {
            result = mid; // Update result inside the else block
            end = mid - 1;
        }
    }
    return result; // Return the final result after the binary search
}
```

63 prblem link https://www.naukri.com/code360/problems/minimise-max-distance_7541449
