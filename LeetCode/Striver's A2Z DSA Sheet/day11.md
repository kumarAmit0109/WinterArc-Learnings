# 51. Find Peak Element

### Problem Link

[Find Peak Element - LeetCode](https://leetcode.com/problems/find-peak-element/description/)

## Approach 1: Linear Search

### Idea

Traverse the array and check for a peak element at each index.

### Steps:

1. For every index `i`, check if the following condition holds:
   - `((i == 0 or arr[i-1] < arr[i]) and (i == n-1 or arr[i] > arr[i+1]))`
2. If the condition is true for any element, return its index.

### Time Complexity

- `O(n)` as we may need to traverse the entire array.

### Space Complexity

- `O(1)` since no extra space is used.

### Code

```cpp
int findPeakElement(vector<int>& nums) {
    int n = nums.size();
    for (int i = 0; i < n; i++) {
        if ((i == 0 || nums[i - 1] < nums[i]) && (i == n - 1 || nums[i] > nums[i + 1])) {
            return i;
        }
    }
    return -1; // Just to satisfy the return type; it should never reach here
}
```

## Approach 2: Binary Search

### Idea

Use binary search to efficiently find a peak element.

### Steps:

1. **Edge Cases:**

   - If the array contains only one element, return `0`.
   - If the first element is greater than the second, return `0`.
   - If the last element is greater than the second last, return `n-1`.

2. **Binary Search:**
   - Initialize two pointers: `low = 1` and `high = n - 2`.
   - While `low <= high`:
     - Calculate `mid = (low + high) / 2`.
     - If `nums[mid - 1] < nums[mid] && nums[mid] > nums[mid + 1]`, return `mid`.
     - If `nums[mid] > nums[mid - 1]`, move to the right half by setting `low = mid + 1`.
     - Otherwise, move to the left half by setting `high = mid - 1`.

### Time Complexity

- `O(log n)` due to the binary search process.

### Space Complexity

- `O(1)` as no extra space is used.

### Code

```cpp
int findPeakElement(vector<int>& nums) {
    int n = nums.size();
    // Edge case: If it contains one element
    if (n == 1) {
        return 0;
    }
    // If the first element is peak
    if (nums[0] > nums[1]) {
        return 0;
    } else if (nums[n - 1] > nums[n - 2]) { // If last element is peak
        return n - 1;
    }

    // Binary search in the rest of the array
    int low = 1, high = n - 2;
    while (low <= high) {
        int mid = (low + high) / 2;
        if (nums[mid - 1] < nums[mid] && nums[mid] > nums[mid + 1]) {
            return mid;
        } else if (nums[mid] > nums[mid - 1]) {
            low = mid + 1;
        } else {
            high = mid - 1;
        }
    }
    return -1; // Just to satisfy the return type; it should never reach here
}
```

# 52. Square Root Integral

### Problem Link

[Square Root Integral - Naukri](https://www.naukri.com/code360/problems/square-root-integral_893351?)

## Approach 1: Linear Search

### Idea

Iterate from `1` to `n` and find the largest integer `i` such that `i * i` is less than or equal to `n`.

### Steps:

1. Declare a variable called `ans`.
2. Run a loop from `1` to `n`:
   - While `i * i <= n`, update `ans` with `i`.
3. Break out of the loop when `i * i` exceeds `n`.
4. Return `ans` as the result.

### Time Complexity

- `O(n)` as we may need to traverse the entire range.

### Space Complexity

- `O(1)` since no extra space is used.

### Code

```cpp
int floorSqrt(int n) {
    int ans = 0;
    for (int i = 1; i <= n; i++) {
        if (i * i <= n) {
            ans = i;
        } else {
            break;
        }
    }
    return ans;
}
```

## Approach 2: Built-in Function

### Idea

Use the built-in `sqrt()` function to efficiently find the square root of `n` and return its floor value.

### Steps:

1. Return the integer value of the square root of `n` using the `sqrt()` function from the `<cmath>` library.

### Time Complexity

- `O(1)` since it directly calls a built-in function.

### Space Complexity

- `O(1)` as no additional space is used.

### Code

```cpp
#include <cmath>

int floorSqrt(int n) {
    return static_cast<int>(sqrt(n)); // Return the floor value of the square root
}
```

## Approach 3: Binary Search

### Idea

Use binary search to find the floor value of the square root of `n`.

### Steps:

1. Declare a variable `ans` to store the potential answer.
2. Initialize two pointers: `low = 1` and `high = n`.
3. While `low` is less than or equal to `high`:
   - Calculate `mid` using the formula: `mid = (low + high) // 2`.
   - If `mid * mid` is less than or equal to `n`:
     - Update `ans` to `mid` (potential answer).
     - Move to the right half by setting `low = mid + 1`.
   - Otherwise, move to the left half by setting `high = mid - 1`.
4. Return `ans` as the final result.

### Time Complexity

- `O(log n)` due to the binary search process.

### Space Complexity

- `O(1)` as no extra space is used.

### Code

```cpp
int floorSqrt(int n) {
    int ans = 0; // Variable to store the potential answer
    int low = 1, high = n;

    while (low <= high) {
        int mid = (low + high) / 2; // Calculate mid
        if (mid * mid <= n) { // Check if mid is a valid answer
            ans = mid; // Update potential answer
            low = mid + 1; // Move to the right half
        } else {
            high = mid - 1; // Move to the left half
        }
    }
    return ans; // Return the final answer
}
```

# 53. Nth Root of M

### Problem Link

[Nth Root of M - Naukri](https://www.naukri.com/code360/problems/nth-root-of-m_1062679?)

## Approach 1: Linear Search

### Idea

Run a loop to find the integer `x` such that \( x^n = m \).

### Steps:

1. For each integer `x` from `1` to `m`, check:
   - If \( x^n == m \), return `x`.
   - If \( x^n > m \), return `-1` as no valid `x` exists.

### Time Complexity

- `O(m^n)` in the worst case for checking each integer up to `m`.

### Space Complexity

- `O(1)` as no extra space is used.

### Code

```cpp
int NthRoot(int n, int m) {
    for (int x = 1; x <= m; x++) {
        if (pow(x, n) == m) {
            return x;
        }
        if (pow(x, n) > m) {
            return -1;
        }
    }
    return -1; // Just to satisfy the return type; it should never reach here
}
```

## Approach 2: Binary Search

### Idea

Use binary search to efficiently find the integer `x` such that \( x^n = m \).

### Steps:

1. **Initialize Pointers:**

   - Set `low` to `1` and `high` to `m`.

2. **Binary Search Logic:**

   - While `low <= high`:
     - Calculate `mid = (low + high) // 2`.
     - If \( mid^n == m \), return `mid`.
     - If \( mid^n < m \), update `low = mid + 1` to search in the right half.
     - If \( mid^n > m \), update `high = mid - 1` to search in the left half.

3. **Return Statement:**
   - If no answer is found, return `-1`.

### Time Complexity

- `O(n log m)` due to binary search and power calculation.

### Space Complexity

- `O(1)` as no extra space is used.

### Code

```cpp
int NthRoot(int n, int m) {
    int low = 1, high = m;
    while (low <= high) {
        int mid = (low + high) / 2;
        if (pow(mid, n) == m) {
            return mid;
        } else if (pow(mid, n) < m) {
            low = mid + 1;
        } else {
            high = mid - 1;
        }
    }
    return -1; // If no valid root exists
}
```

# 54. Koko Eating Bananas

### Problem Link

[Koko Eating Bananas - LeetCode](https://leetcode.com/problems/koko-eating-bananas/description/)

## Approach 1: Naive Approach (Brute-force)

### Idea

Check all possible eating speeds from 1 to `max(a[])`. The minimum speed for which the required time is less than or equal to `h` is our answer.

### Steps:

1. Find the maximum value in the given array, `max(a[])`.
2. Run a loop from `1` to `max(a[])` to check all possible eating speeds.
3. For each speed `i`, calculate the hours required to consume all the bananas using the function `calculateTotalHours(a[], i)`.
4. Return the first speed `i` for which the required hours are less than or equal to `h`.

### calculateTotalHours(a[], hourly):

- Iterate through each pile in the given array.
- For each pile, calculate the hours required using `ceil((double)bananas / hourly)` and add it to the total hours.
- Return the total hours.

### Time Complexity

- `O(n * max(a[]))`, where `n` is the number of piles.

### Space Complexity

- `O(1)` as no extra space is used.

### Code

```cpp
long long calculateTotalHours(vector<int>& piles, int hourly) {
    long long totalHours = 0; // Change to long long to avoid overflow
    for (int bananas : piles) {
        totalHours += ceil((double)bananas / hourly); // Use double to avoid integer division
    }
    return totalHours;
}

int minEatingSpeed(vector<int>& piles, int h) {
    int maxBananas = *max_element(piles.begin(), piles.end());
    for (int i = 1; i <= maxBananas; ++i) {
        if (calculateTotalHours(piles, i) <= h) {
            return i;
        }
    }
    return -1; // Just to satisfy the return type; it should never reach here
}
```

## Approach 2: Binary Search

### Idea

Use binary search to find the minimum eating speed efficiently.

### Steps:

1. Find the maximum element in the given array, `max(a[])`.
2. Initialize two pointers: `low` set to `1` and `high` set to `max(a[])`.
3. While `low <= high`:
   - Calculate `mid = (low + high) // 2`.
   - Calculate the total hours required to eat at the speed of `mid` using `calculateTotalHours(a[], mid)`.
   - If `totalH <= h`, update `high = mid - 1` to search for a smaller speed.
   - Otherwise, update `low = mid + 1` to search for a larger speed.
4. Return the value of `low` as it will point to the minimum eating speed.

### calculateTotalHours(a[], hourly):

- Iterate through each pile in the given array.
- For each pile, calculate the hours required using `ceil((double)bananas / hourly)` and add it to the total hours.
- Return the total hours.

### Time Complexity

- `O(n log(max(a[])))` due to the binary search process.

### Space Complexity

- `O(1)` as no extra space is used.

### Code

```cpp
long long calculateTotalHours(vector<int>& piles, int hourly) {
    long long totalHours = 0; // Change to long long to avoid overflow
    for (int bananas : piles) {
        totalHours += ceil((double)bananas / hourly); // Use double to avoid integer division
    }
    return totalHours;
}

int minEatingSpeed(vector<int>& piles, int h) {
    int maxBananas = *max_element(piles.begin(), piles.end());
    int low = 1, high = maxBananas;

    while (low <= high) {
        int mid = (low + high) / 2;
        long long totalH = calculateTotalHours(piles, mid); // Use long long to avoid overflow

        if (totalH <= h) {
            high = mid - 1; // Try for a smaller speed
        } else {
            low = mid + 1; // Try for a larger speed
        }
    }

    return low; // Low points to the minimum eating speed
}
```
