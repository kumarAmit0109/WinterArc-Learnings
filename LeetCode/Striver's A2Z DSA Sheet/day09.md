# 41. Lower Bound

### Problem Link

[Naukri - Lower Bound](https://www.naukri.com/code360/problems/lower-bound_8165382)

## Approach 1: Linear Search

1. **Idea**: Traverse the array starting from the first element. Compare each element with the target `x`. The first index where `arr[i] >= x` is the answer.

2. **Steps**:

   - Traverse the array from the beginning.
   - For each element, check if it is greater than or equal to `x`.
   - Return the index if the condition is met; otherwise, return `n` (size of the array) if no such element is found.

3. **Time Complexity**: `O(n)`  
   We traverse the array once.

4. **Space Complexity**: `O(1)`  
   No extra space is used.

### Code

```cpp
int lowerBound(vector<int> arr, int n, int x) {
    for (int i = 0; i < n; i++) {
        if (arr[i] >= x) {
            return i;
        }
    }
    return n; // If no element is found, return n
}
```

## Approach 2: Binary Search

1. **Idea**: Since the array is sorted, we use binary search to find the index where `arr[mid] >= x`.

2. **Steps**:

   - Initialize two pointers, `low` and `high`, where `low` points to the first element and `high` to the last element.
   - Initialize `ans` to `n` (size of the array) because if no such index is found, we will return `n`.
   - Compute the middle index `mid = (low + high) / 2`.
   - If `arr[mid] >= x`, update `ans` with `mid` and move the `high` pointer to search in the left half.
   - If `arr[mid] < x`, move the `low` pointer to search in the right half.
   - Continue this process until `low` crosses `high`.

3. **Time Complexity**: `O(log n)`  
   Binary search divides the array in half at each step, so the time complexity is logarithmic.

4. **Space Complexity**: `O(1)`  
   Binary search requires a constant amount of extra space.

### Code

```cpp
int lowerBound(vector<int> arr, int n, int x) {
    int low = 0, high = n - 1, ans = n;

    while (low <= high) {
        int mid = (low + high) / 2;

        if (arr[mid] >= x) {
            ans = mid;  // Found a valid index, store it
            high = mid - 1;  // Move left to find smaller index
        } else {
            low = mid + 1;  // Move right to find a larger value
        }
    }

    return ans; // Return the first index where arr[i] >= x or n if not found
}
```

# 42. Upper Bound

### Problem Link

[Naukri - Upper Bound](https://www.naukri.com/code360/problems/implement-upper-bound_8165383)

## Approach 1: Linear Search

1. **Idea**: Traverse the array from the beginning and compare each element with the target `x`. The first index where `arr[i] > x` is the answer.

2. **Steps**:

   - Traverse the array starting from index 0.
   - For each element, check if it is greater than `x`.
   - Return the index if the condition is met; otherwise, return `n` (the size of the array) if no such element is found.

3. **Time Complexity**: `O(n)`  
   We traverse the array once.

4. **Space Complexity**: `O(1)`  
   No extra space is used.

### Code

```cpp
int upperBound(vector<int> &arr, int x, int n) {
    for (int i = 0; i < n; i++) {
        if (arr[i] > x) {
            return i;
        }
    }
    return n; // If no element is greater than x, return n
}
```

## Approach 2: Binary Search

1. **Idea**: Since the array is sorted, we use binary search to efficiently find the index where `arr[mid] > x`.

2. **Steps**:

   - Initialize two pointers, `low` at the first index and `high` at the last index.
   - Initialize `ans` to `n` (size of the array) because if no such index is found, we will return `n`.
   - Compute the middle index `mid = (low + high) / 2`.
   - If `arr[mid] > x`, update `ans` with `mid` and move the `high` pointer to search in the left half.
   - If `arr[mid] <= x`, move the `low` pointer to search in the right half.
   - Continue this process until `low` crosses `high`.

3. **Time Complexity**: `O(log n)`  
   Binary search divides the array in half at each step, making it logarithmic in time.

4. **Space Complexity**: `O(1)`  
   Binary search requires constant extra space.

### Code

```cpp
int upperBound(vector<int> &arr, int x, int n) {
    int low = 0, high = n - 1, ans = n;

    while (low <= high) {
        int mid = (low + high) / 2;

        if (arr[mid] > x) {
            ans = mid;  // Found a valid index, store it
            high = mid - 1;  // Move left to find a smaller index
        } else {
            low = mid + 1;  // Move right to find a larger value
        }
    }

    return ans; // Return the first index where arr[i] > x or n if not found
}
```

# 43 Search Insert Position

### Problem Link

[Leetcode - Search Insert Position](https://leetcode.com/problems/search-insert-position/description/)

## Approach 1: Binary Search

1. **Idea**: We use binary search to efficiently find the position to insert the `target` value. If the `target` is already present, we return its index; otherwise, we return the position where it would be inserted in the sorted array.

2. **Steps**:

   - Initialize two pointers, `strt` (start) at the first index and `end` at the last index of the array.
   - Set the default `ans` to the size of the array, assuming the target might be inserted at the end.
   - Compute the middle index `mid = strt + (end - strt) / 2`.
   - Compare `nums[mid]` with the `target`:
     - If `nums[mid] == target`, return `mid` (exact match found).
     - If `nums[mid] < target`, move the `strt` pointer to search in the right half.
     - If `nums[mid] > target`, update `ans` to `mid` and move the `end` pointer to search in the left half.
   - Continue the process until `strt` crosses `end`.
   - Finally, return `ans`, which will be the index where `target` would be inserted.

3. **Time Complexity**: `O(log n)`  
   Binary search divides the array in half at each step, making it logarithmic in time.

4. **Space Complexity**: `O(1)`  
   The algorithm uses constant extra space.

### Code

```cpp
int searchInsert(vector<int>& nums, int target) {
    int strt = 0, end = nums.size() - 1;
    int ans = nums.size();  // Default to the end of the array

    while (strt <= end) {
        int mid = strt + (end - strt) / 2;

        if (nums[mid] == target) {
            return mid;  // Found an exact match, return the index
        } else if (nums[mid] < target) {
            strt = mid + 1;
        } else {
            ans = mid;  // Update ans but continue searching in the left half
            end = mid - 1;
        }
    }

    return ans;
}
```

# 44. Find First and Last Position of Element in Sorted Array

### Problem Link

[Leetcode - Find First and Last Position of Element in Sorted Array](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/description/)

## Approach 1: Brute Force (Linear Search)

1. **Idea**: We can iterate over the array to find the first occurrence of the target by traversing from the start. To find the last occurrence, we can traverse the array starting from the end. This would provide both indices.

2. **Steps**:

   - Start from index `0` and search until the first occurrence of the `target` is found.
   - Then, start from the last index and move backward to find the last occurrence of the `target`.

3. **Time Complexity**: `O(n)`  
   We may need to traverse the entire array twice in the worst case.

4. **Space Complexity**: `O(1)`  
   No extra space is used except for the result array.

### Code

```cpp
vector<int> searchRange(vector<int>& nums, int target) {
    vector<int> ans(2, -1);

    // Find first occurrence
    for (int i = 0; i < nums.size(); i++) {
        if (nums[i] == target) {
            ans[0] = i;  // First occurrence
            break;  // Exit the loop after finding the first occurrence
        }
    }

    // Find last occurrence
    for (int i = nums.size() - 1; i >= 0; i--) {
        if (nums[i] == target) {
            ans[1] = i;  // Last occurrence
            break;  // Exit the loop after finding the last occurrence
        }
    }

    return ans;
}
```

## Approach 2: Binary Search (Optimized)

1. **Idea**: Since the array is sorted, we can use binary search to find both the first and last occurrences of the `target`. First, we search for the leftmost occurrence of the target, then for the rightmost occurrence.

2. **Steps**:

   - First, we perform a binary search to find the leftmost occurrence:
     - If `nums[mid] == target`, we update the answer and continue searching in the left half (`right = mid - 1`) to ensure we find the first occurrence.
   - Then, we perform a second binary search to find the rightmost occurrence:
     - If `nums[mid] == target`, we update the answer and continue searching in the right half (`left = mid + 1`) to ensure we find the last occurrence.
   - If no such element exists, return `[-1, -1]`.

3. **Time Complexity**: `O(log n)`  
   We perform two binary searches, each taking logarithmic time.

4. **Space Complexity**: `O(1)`  
   The algorithm uses a constant amount of extra space.

### Code

```cpp
vector<int> searchRange(vector<int>& nums, int target) {
    vector<int> ans(2, -1);

    // Search for the leftmost occurrence
    int left = 0, right = nums.size() - 1;
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] == target) {
            ans[0] = mid;
            right = mid - 1;  // Continue searching in the left half
        } else if (nums[mid] > target) {
            right = mid - 1;
        } else {
            left = mid + 1;
        }
    }

    // Reset for the rightmost occurrence
    left = 0;
    right = nums.size() - 1;

    // Search for the rightmost occurrence
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] == target) {
            ans[1] = mid;
            left = mid + 1;  // Continue searching in the right half
        } else if (nums[mid] > target) {
            right = mid - 1;
        } else {
            left = mid + 1;
        }
    }

    return ans;
}
```

# 45. Occurrence of X in a Sorted Array

### Problem Link

[Naukri - Occurrence of X in a Sorted Array](https://www.naukri.com/code360/problems/occurrence-of-x-in-a-sorted-array_630456?)

## Approach 1: Binary Search (Optimized)

1. **Idea**: Since the array is sorted, we can use binary search to find both the first and last occurrences of the target value, `x`. We can then calculate the number of occurrences based on these indices.

2. **Steps**:

   - First, we perform a binary search to find the leftmost occurrence of `x`.
     - If we find `x`, we store its index and continue searching in the left half to ensure we find the first occurrence.
   - Next, we perform another binary search to find the rightmost occurrence of `x`.
     - Again, if we find `x`, we store its index and continue searching in the right half to ensure we find the last occurrence.
   - If the leftmost or rightmost occurrence is not found, we return `0` as the count.
   - Finally, we calculate the count as `last - first + 1`.

3. **Time Complexity**: `O(log n)`  
   We perform two binary searches, each taking logarithmic time.

4. **Space Complexity**: `O(1)`  
   The algorithm uses a constant amount of extra space.

### Code

```cpp
vector<int> searchRange(vector<int>& nums, int target) {
    vector<int> ans(2, -1);

    // Search for the leftmost occurrence
    int left = 0, right = nums.size() - 1;
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] == target) {
            ans[0] = mid;
            right = mid - 1;  // Continue searching in the left half
        } else if (nums[mid] > target) {
            right = mid - 1;
        } else {
            left = mid + 1;
        }
    }

    // Reset for the rightmost occurrence
    left = 0;
    right = nums.size() - 1;

    // Search for the rightmost occurrence
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] == target) {
            ans[1] = mid;
            left = mid + 1;  // Continue searching in the right half
        } else if (nums[mid] > target) {
            right = mid - 1;
        } else {
            left = mid + 1;
        }
    }

    return ans;
}

int count(vector<int>& arr, int n, int x) {
    // Write Your Code Here
    vector<int> ans(2);
    ans = searchRange(arr, x);
    if (ans[0] == -1 || ans[1] == -1) {
        return 0;  // Return 0 if x is not found
    }
    return ans[1] - ans[0] + 1;  // Calculate the number of occurrences
}
```
