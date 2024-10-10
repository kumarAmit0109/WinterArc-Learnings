# 46. Search in Rotated Sorted Array

### Problem Link

[Search in Rotated Sorted Array - LeetCode](https://leetcode.com/problems/search-in-rotated-sorted-array/description/)

## Approach 1: Naive Linear Search

### Idea

In this approach, we simply traverse through the array and check if the current element matches the target. If we find the target, we return its index. Otherwise, return `-1` after the loop finishes.

### Steps:

1. Traverse through the entire array.
2. For each element, check if it equals the target.
3. If found, return its index.
4. If not found after the traversal, return `-1`.

### Time Complexity

- `O(n)` where `n` is the size of the array.

### Space Complexity

- `O(1)` as we use constant space.

### Code

```cpp
int search(vector<int>& nums, int target) {
    for (int i = 0; i < nums.size(); i++) {
        if (nums[i] == target) {
            return i;
        }
    }
    return -1;
}
```

## Approach 2: Optimized Binary Search (Considering Rotation)

### Idea

In this approach, we use a binary search to take advantage of the sorted nature of the array. First, we check the middle element and identify which part of the array (left or right) is sorted. Depending on this, we adjust our search to the appropriate half of the array.

### Steps:

1. Find the middle element.
2. Check if the target is equal to the middle element.
3. Identify which half of the array is sorted:
   - If the left half is sorted (i.e., `nums[start] <= nums[mid]`):
     - If the target lies within the left half (i.e., `nums[start] <= target <= nums[mid]`), adjust the `end` pointer.
     - Otherwise, adjust the `start` pointer.
   - If the right half is sorted (i.e., `nums[mid] <= nums[end]`):
     - If the target lies within the right half (i.e., `nums[mid] <= target <= nums[end]`), adjust the `start` pointer.
     - Otherwise, adjust the `end` pointer.
4. Continue this process until the target is found or the search space is exhausted.

### Time Complexity

- `O(log n)` where `n` is the size of the array.

### Space Complexity

- `O(1)` as we use constant space.

### Code

```cpp
int search(vector<int>& nums, int target) {
    int strt = 0, end = nums.size() - 1;
    while (strt <= end) {
        int mid = strt + (end - strt) / 2;
        // Target found
        if (nums[mid] == target) {
            return mid;
        }
        // Left half is sorted
        if (nums[strt] <= nums[mid]) {
            if (nums[strt] <= target && target <= nums[mid]) {
                end = mid - 1;
            } else {
                strt = mid + 1;
            }
        }
        // Right half is sorted
        else {
            if (nums[mid] <= target && target <= nums[end]) {
                strt = mid + 1;
            } else {
                end = mid - 1;
            }
        }
    }
    return -1;
}
```

## Approach 3: Pivot + Binary Search

### Idea

This approach divides the problem into two parts:

1. **Find the pivot (smallest element)**, which is the point where the array is rotated.
2. **Perform binary search** on either side of the pivot to find the target.

### Steps:

1. **Finding the Pivot**:
   - We use binary search to find the smallest element (pivot) in the rotated array.
   - The pivot is the point where the array goes from decreasing back to increasing, indicating the start of the rotation.
2. **Binary Search on the Appropriate Side**:
   - Once the pivot is identified, we know that the array is sorted from the pivot to the end and from the start to just before the pivot.
   - We perform binary search either in the left part (before the pivot) or in the right part (after the pivot) based on the target's value.

### Time Complexity

- `O(log n)` for finding the pivot and another `O(log n)` for binary search, so overall `O(log n)`.

### Space Complexity

- `O(1)` as we use constant space.

### Code

```cpp
int getPivot(vector<int>& nums) {
    int s = 0, e = nums.size() - 1;
    int mid = s + (e - s) / 2;
    while (s < e) {
        mid = s + (e - s) / 2;
        if (nums[mid] >= nums[0]) {
            s = mid + 1;
        } else {
            e = mid;
        }
    }
    return s;
}

bool binarySearch(vector<int>& nums, int strt, int end, int target) {
    while (strt <= end) {
        int mid = strt + (end - strt) / 2;
        if (nums[mid] == target) {
            return mid;
        } else if (nums[mid] < target) {
            strt = mid + 1;
        } else {
            end = mid - 1;
        }
    }
    return -1;
}

int search(vector<int>& nums, int target) {
    int pivot = getPivot(nums);
    if (target >= nums[pivot] && target <= nums[nums.size() - 1]) {
        return binarySearch(nums, pivot, nums.size() - 1, target);
    } else {
        return binarySearch(nums, 0, pivot - 1, target);
    }
}
```

# 47. Search in Rotated Sorted Array II

### Problem Link

[Search in Rotated Sorted Array II - LeetCode](https://leetcode.com/problems/search-in-rotated-sorted-array-ii/)

## Approach 1: Linear Search

### Idea

We perform a simple linear search by traversing the array and checking each element to see if it equals the target.

### Steps:

1. Traverse the array one element at a time.
2. If any element matches the target, return `True`.
3. If no element matches the target after completing the traversal, return `False`.

### Time Complexity

- `O(n)` since we may need to check all elements.

### Space Complexity

- `O(1)` as no extra space is used.

### Code

```cpp
bool search(vector<int>& nums, int target) {
    for (int i = 0; i < nums.size(); i++) {
        if (nums[i] == target) {
            return true;
        }
    }
    return false;
}
```

## Approach 2: Binary Search (Handling Duplicates)

### Idea

This approach extends the binary search method by handling duplicates. Since the array is rotated and contains duplicates, we use a modified binary search to account for these conditions.

### Steps:

1. Find the middle element of the array.
2. If the middle element matches the target, return `True`.
3. If there are duplicates at the start, middle, and end, skip those by incrementing the start and decrementing the end.
4. Otherwise, determine whether the left half or the right half of the array is sorted and apply binary search accordingly:
   - If the left half is sorted and the target lies within the range of this half, search the left half.
   - If the right half is sorted and the target lies within the range of this half, search the right half.

### Time Complexity

- Worst-case time complexity is `O(n)` due to the presence of duplicates, which can force skipping elements.

### Space Complexity

- `O(1)` since binary search is performed iteratively.

### Code

```cpp
bool search(vector<int>& nums, int target) {
    int strt = 0, end = nums.size() - 1;
    while (strt <= end) {
        int mid = strt + (end - strt) / 2;
        if (nums[mid] == target) {
            return true;
        }
        // Skip duplicates
        if (nums[strt] == nums[mid] && nums[mid] == nums[end]) {
            strt++;
            end--;
            continue;
        }
        // Check which half is sorted
        if (nums[strt] <= nums[mid]) {
            if (nums[strt] <= target && target <= nums[mid]) {
                end = mid - 1;
            } else {
                strt = mid + 1;
            }
        } else {
            if (nums[mid] <= target && target <= nums[end]) {
                strt = mid + 1;
            } else {
                end = mid - 1;
            }
        }
    }
    return false;
}
```

# 48. Find Minimum in Rotated Sorted Array

### Problem Link

[Find Minimum in Rotated Sorted Array - LeetCode](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/description/)

## Approach 1: Linear Search

### Idea

We perform a simple linear search by traversing the array and keeping track of the minimum value encountered.

### Steps:

1. Declare a variable `mini` initialized to a large value.
2. Traverse the array one element at a time.
3. Update `mini` with the minimum value using the formula: `mini = min(mini, arr[i])`.
4. Return `mini` as the final answer.

### Time Complexity

- `O(n)` since we may need to check all elements.

### Space Complexity

- `O(1)` as no extra space is used.

### Code

```cpp
int findMin(vector<int>& arr) {
    int mini = INT_MAX;  // Initialize mini with a large value
    for (int i = 0; i < arr.size(); i++) {
        mini = min(mini, arr[i]);  // Update mini with the minimum value
    }
    return mini;  // Return the minimum value found
}
```

## Approach 2: Binary Search

### Idea

This approach utilizes a binary search method to find the minimum element in a rotated sorted array efficiently.

### Steps:

1. Initialize two pointers, `low` and `high`, to point to the first and last indices of the array, respectively.
2. Initialize an `ans` variable with the largest possible value.
3. Inside a loop, calculate the `mid` index using the formula: `mid = (low + high) / 2`.
4. If the left half (from `low` to `mid`) is sorted:
   - Update `ans` with the minimum value found so far using `ans = min(ans, arr[low])`.
   - Search in the right half by updating `low = mid + 1`.
5. If the right half (from `mid` to `high`) is sorted:
   - Update `ans` with the minimum value found so far using `ans = min(ans, arr[mid])`.
   - Search in the left half by updating `high = mid - 1`.
6. Repeat steps 3-5 until `low` crosses `high`.
7. Return the `ans` variable that stores the minimum element.

### Time Complexity

- `O(log n)` due to the binary search method.

### Space Complexity

- `O(1)` as no extra space is used.

### Code

```cpp
int findMin(vector<int>& arr) {
    int low = 0, high = arr.size() - 1;
    int ans = INT_MAX;  // Initialize ans with a large value

    while (low <= high) {
        int mid = (low + high) / 2;

        if (arr[low] <= arr[mid]) {
            ans = min(ans, arr[low]);
            low = mid + 1;  // Search in the right half
        } else {
            ans = min(ans, arr[mid]);
            high = mid - 1;  // Search in the left half
        }
    }

    return ans;  // Return the minimum value found
}
```

## Approach 3: Optimized Binary Search

### Idea

This approach optimizes the binary search technique to find the minimum element while handling the conditions for sorted halves and duplicates.

### Steps:

1. Initialize two pointers, `low` and `high`, to point to the first and last indices of the array, respectively.
2. Initialize an `ans` variable with the largest possible value.
3. Inside a loop, calculate the `mid` index using the formula: `mid = (low + high) // 2`.
4. If `arr[low] <= arr[high]`, it means the array from `low` to `high` is completely sorted:
   - Update `ans` with the minimum value found so far using `ans = min(ans, arr[low])`.
   - Break out of the loop as the minimum is found.
5. Identify which half is sorted:
   - If `arr[low] <= arr[mid]`, the left half is sorted:
     - Update `ans` using `ans = min(ans, arr[low])`.
     - Eliminate the left half by updating `low = mid + 1`.
   - Otherwise, if the right half is sorted:
     - Update `ans` using `ans = min(ans, arr[mid])`.
     - Eliminate the right half by updating `high = mid - 1`.
6. Repeat steps 3-5 until `low` crosses `high`.
7. Return the `ans` variable that stores the minimum element.

### Time Complexity

- `O(log n)` due to the binary search method.

### Space Complexity

- `O(1)` as no extra space is used.

### Code

```cpp
int findMin(vector<int>& arr) {
    int low = 0, high = arr.size() - 1;
    int ans = INT_MAX;  // Initialize ans with a large value

    while (low <= high) {
        int mid = (low + high) / 2;

        // If the array from low to high is completely sorted
        if (arr[low] <= arr[high]) {
            ans = min(ans, arr[low]);  // Update ans with the minimum value
            break;  // Break out of the loop
        }

        // Identify which half is sorted and update ans
        if (arr[low] <= arr[mid]) {
            ans = min(ans, arr[low]);
            low = mid + 1;  // Eliminate the left half
        } else {
            ans = min(ans, arr[mid]);
            high = mid - 1;  // Eliminate the right half
        }
    }

    return ans;  // Return the minimum value found
}
```

# 49. Find K Rotation (Index of Minimum Element in Rotated Sorted Array)

### Problem Link

[Find K Rotation - Naukri](https://www.naukri.com/code360/problems/rotation_7449070)

## Approach 1: Linear Search

### Idea

We perform a simple linear search by iterating through the array to find the minimum element and its index.

### Steps:

1. Declare an `ans` variable initialized with a large number and an `index` variable initialized with -1.
2. Iterate through the array:
   - If any element `arr[i]` is smaller than `ans`, update `ans` with `arr[i]` and `index` with `i`.
3. Return the `index` variable as the result.

### Time Complexity

- `O(n)` since we may need to check all elements.

### Space Complexity

- `O(1)` as no extra space is used.

### Code

```cpp
int findKRotation(vector<int> &arr) {
    int ans = INT_MAX;
    int index = -1;

    for (int i = 0; i < arr.size(); i++) {
        if (arr[i] < ans) {
            ans = arr[i];
            index = i;
        }
    }
    return index;
}
```

## Approach 2: Binary Search

### Idea

This approach utilizes a binary search method to efficiently find the index of the minimum element in a rotated sorted array.

### Steps:

1. Declare an `ans` variable initialized with the largest possible value, and an `index` variable initialized with -1.
2. Initialize two pointers, `low` and `high`, to point to the first and last indices of the array, respectively.
3. Inside a loop, calculate the `mid` index using the formula: `mid = (low + high) / 2`.
4. If `arr[low] <= arr[high]`, the array from `low` to `high` is completely sorted:
   - If `arr[low] < ans`, update `ans` and `index` with `arr[low]` and `low`, respectively.
   - Break the loop.
5. If `arr[low] <= arr[mid]` (left half is sorted):
   - If `arr[low] < ans`, update `ans` and `index` with `arr[low]` and `low`.
   - Eliminate the left half by updating `low = mid + 1`.
6. Otherwise (right half is sorted):
   - If `arr[mid] < ans`, update `ans` and `index` with `arr[mid]` and `mid`.
   - Eliminate the right half by updating `high = mid - 1`.
7. Continue until `low` crosses `high`.
8. Return the `index` variable.

### Time Complexity

- `O(log n)` due to the binary search method.

### Space Complexity

- `O(1)` as no extra space is used.

### Code

```cpp
int findKRotation(vector<int> &arr) {
    int ans = INT_MAX;
    int index = -1;
    int low = 0, high = arr.size() - 1;

    while (low <= high) {
        int mid = (low + high) / 2;

        if (arr[low] <= arr[high]) {
            if (arr[low] < ans) {
                ans = arr[low];
                index = low;
            }
            break;  // No need to continue binary search
        }

        if (arr[low] <= arr[mid]) { // Left half is sorted
            if (arr[low] < ans) {
                ans = arr[low];
                index = low;
            }
            low = mid + 1;  // Search in the right half
        } else { // Right half is sorted
            if (arr[mid] < ans) {
                ans = arr[mid];
                index = mid;
            }
            high = mid - 1;  // Search in the left half
        }
    }

    return index;  // Return the index of the minimum element
}
```

# 50. Single Element in a Sorted Array

### Problem Link

[Single Element in a Sorted Array - LeetCode](https://leetcode.com/problems/single-element-in-a-sorted-array/description/)

## Approach 1: Linear Search

### Idea

We traverse the array and check the surrounding elements to find the single element.

### Steps:

1. If the array contains only one element, return that element.
2. Traverse the array:
   - If at the first index (`i == 0`), check if the next element is equal. If not, return `arr[i]`.
   - If at the last index (`i == n-1`), check if the previous element is equal. If not, return `arr[i]`.
   - For other elements, if `arr[i] != arr[i-1]` and `arr[i] != arr[i+1]`, return `arr[i]`.

### Time Complexity

- `O(n)` as we traverse the entire array.

### Space Complexity

- `O(1)` as no extra space is used.

### Code

```cpp
int singleNonDuplicate(vector<int>& nums) {
    if (nums.size() == 1) return nums[0];

    for (int i = 0; i < nums.size(); i++) {
        if (i == 0) {
            if (nums[i] != nums[i + 1]) return nums[i];
        } else if (i == nums.size() - 1) {
            if (nums[i] != nums[i - 1]) return nums[i];
        } else {
            if (nums[i] != nums[i - 1] && nums[i] != nums[i + 1]) {
                return nums[i];
            }
        }
    }
    return -1; // This line should never be reached
}
```

## Approach 2: XOR

### Idea

Utilize the XOR operation to find the single element, as the same elements will cancel each other out.

### Steps:

1. Declare an `ans` variable initialized with `0`.
2. Traverse the array and XOR each element with `ans`.
3. After complete traversal, return `ans`, which will hold the single element.

### Time Complexity

- `O(n)` due to the traversal of the array.

### Space Complexity

- `O(1)` as no extra space is used.

### Code

```cpp
int singleNonDuplicate(vector<int>& nums) {
    int ans = 0;
    for (int num : nums) {
        ans ^= num;
    }
    return ans;
}
```

## Approach 3: Binary Search

### Idea

Use a binary search technique to find the single element by narrowing down the search space based on the properties of the array.

### Steps:

1. If `n == 1`, return the only element in the array.
2. If `arr[0] != arr[1]`, return `arr[0]` as the single element.
3. If `arr[n-1] != arr[n-2]`, return `arr[n-1]` as the single element.
4. Initialize two pointers, `low` at `1` and `high` at `n-2`.
5. Inside a loop:
   - Calculate `mid = (low + high) // 2`.
   - If `arr[mid]` is the single element (i.e., `arr[mid] != arr[mid-1]` and `arr[mid] != arr[mid+1]`), return `arr[mid]`.
   - If `(mid % 2 == 0 && arr[mid] == arr[mid + 1])` or `(mid % 2 == 1 && arr[mid] == arr[mid - 1])`, eliminate the left half by setting `low = mid + 1`.
   - Otherwise, eliminate the right half by setting `high = mid - 1`.

### Time Complexity

- `O(log n)` due to the binary search process.

### Space Complexity

- `O(1)` as no extra space is used.

### Code

```cpp
int singleNonDuplicate(vector<int>& nums) {
    int n = nums.size();
    if (n == 1) return nums[0];
    if (nums[0] != nums[1]) return nums[0];
    if (nums[n - 1] != nums[n - 2]) return nums[n - 1];

    int low = 1, high = n - 2;
    while (low <= high) {
        int mid = (low + high) / 2;
        if (nums[mid] != nums[mid - 1] && nums[mid] != nums[mid + 1]) {
            return nums[mid];
        }
        if ((mid % 2 == 0 && nums[mid] == nums[mid + 1]) ||
            (mid % 2 == 1 && nums[mid] == nums[mid - 1])) {
            low = mid + 1;
        } else {
            high = mid - 1;
        }
    }
    return -1; // Just to satisfy the return type; it should never reach here
}
```
