# 34. Merge Intervals

### Problem Link

[Merge Intervals - LeetCode](https://leetcode.com/problems/merge-intervals/description/)

## Approach 1: Naive Approach (Brute-force)

### Idea

The idea is to merge overlapping intervals by sorting the intervals first and then grouping closer intervals. We use two loops to traverse the array of intervals and check if two intervals can be merged. If they can, we merge them, otherwise, we move to the next interval.

### Steps:

1. **Sort the Intervals**:
   - First, we sort the intervals based on their start time. This ensures that closer intervals will be adjacent.
2. **Merge the Intervals**:
   - Initialize an empty result list to store merged intervals.
   - Iterate through the intervals:
     - If the result list is empty or the current interval cannot be merged with the last interval in the result list, add it to the result.
     - If the current interval overlaps with the last interval in the result list, merge the intervals by updating the end of the last interval to the maximum of the two overlapping intervals.
3. **Return the Result**:
   - Return the merged intervals list.

### Time Complexity

- `O(n log n)` due to sorting the intervals.
- `O(n)` for merging the intervals.

Overall time complexity is `O(n log n)`.

### Space Complexity

- `O(n)` for storing the result.

### Code

```cpp
vector<vector<int>> merge(vector<vector<int>>& intervals) {
    if (intervals.empty()) return {};  // Handle edge case if no intervals

    // Step 1: Sort intervals based on their start time
    sort(intervals.begin(), intervals.end());

    // Step 2: Initialize the result list
    vector<vector<int>> result;

    // Step 3: Traverse the intervals and merge if necessary
    for (auto& interval : intervals) {
        // If result is empty or current interval does not overlap with the last one
        if (result.empty() || result.back()[1] < interval[0]) {
            result.push_back(interval);  // Add the interval to result
        } else {
            // If the intervals overlap, merge them by updating the end of the last interval
            result.back()[1] = max(result.back()[1], interval[1]);
        }
    }

    return result;  // Return the merged intervals
}
```

## Approach 2: Optimized Approach (Single Loop Merge)

### Idea

Instead of using a nested loop to check for merging intervals, we can accomplish this in a single traversal after sorting the intervals. We maintain a result list where we merge intervals as we go through the array.

### Steps:

1. **Sort the Intervals**:

   - Sort the intervals by their start time to ensure closer intervals are adjacent.

2. **Merge Intervals**:
   - Start traversing the sorted intervals.
   - Add the first interval directly to the result.
   - For each subsequent interval:
     - **Case 1**: If it overlaps with the last interval in the result, merge them by updating the last interval's end time to the maximum end time.
     - **Case 2**: If it does not overlap, add it directly to the result list.
3. **Return the Result**:
   - Return the merged intervals from the result list.

### Time Complexity

- Sorting the intervals takes `O(n log n)`.
- Merging intervals takes `O(n)`.

Overall time complexity is `O(n log n)`.

### Space Complexity

- `O(n)` for storing the result.

### Code

```cpp
vector<vector<int>> merge(vector<vector<int>>& intervals) {
    if (intervals.empty()) return {};  // Handle edge case if no intervals

    // Step 1: Sort intervals based on their start time
    sort(intervals.begin(), intervals.end());

    // Step 2: Initialize the result list with the first interval
    vector<vector<int>> result;
    result.push_back(intervals[0]);

    // Step 3: Traverse the intervals and merge as needed
    for (int i = 1; i < intervals.size(); i++) {
        // Get the last interval in the result
        vector<int>& lastInterval = result.back();

        // Case 1: If intervals overlap, merge them
        if (intervals[i][0] <= lastInterval[1]) {
            lastInterval[1] = max(lastInterval[1], intervals[i][1]);
        }
        // Case 2: If no overlap, add the current interval to the result
        else {
            result.push_back(intervals[i]);
        }
    }

    return result;  // Return the merged intervals
}
```

# 35. Merge Two Sorted Arrays Without Extra Space

### Problem Link

[Naukri - Merge Two Sorted Arrays Without Extra Space](https://www.naukri.com/code360/problems/merge-two-sorted-arrays-without-extra-space_6898839?1)

## Approach 1: Use a Third Array

### Idea:

This approach uses an extra array to merge the two input arrays. We use two pointers, one for each array, and compare their elements. The smaller element is added to the new array, and the corresponding pointer is moved forward. Once one of the arrays is fully traversed, the remaining elements of the other array are copied into the third array. Finally, the merged elements are copied back into the original arrays.

### Steps:

1. Initialize two pointers, `left` for array `a[]` and `right` for array `b[]`.
2. Create a third array, `arr3[]`, of size `a.size() + b.size()`.
3. Compare elements pointed by `left` and `right`. Insert the smaller element into `arr3[]`.
4. Continue this process until one of the arrays is fully traversed, then copy the remaining elements.
5. Copy the elements back from `arr3[]` into `a[]` and `b[]`.

### Time and Space Complexity

- **Time Complexity**: `O(n + m)`  
  Merging two arrays takes linear time.
- **Space Complexity**: `O(n + m)`  
  A new array `arr3[]` is created to hold the merged result.

### Code

```cpp
void mergeTwoSortedArraysWithoutExtraSpace(vector<long long> &a, vector<long long> &b) {
    int left = 0, right = 0;
    vector<long long> arr3(a.size() + b.size());
    int index = 0;

    // Merge both arrays into arr3
    while (left < a.size() && right < b.size()) {
        if (a[left] < b[right]) {
            arr3[index++] = a[left++];
        } else {
            arr3[index++] = b[right++];
        }
    }

    // Add remaining elements of a[]
    while (left < a.size()) {
        arr3[index++] = a[left++];
    }

    // Add remaining elements of b[]
    while (right < b.size()) {
        arr3[index++] = b[right++];
    }

    // Copy merged elements back into a[] and b[]
    for (int i = 0; i < a.size(); i++) {
        a[i] = arr3[i];
    }
    for (int i = 0; i < b.size(); i++) {
        b[i] = arr3[i + a.size()];
    }
}
```

## Approach 2: Two-Pointer Swap and Sort

### Idea:

This approach uses two pointers, one pointing at the last element of the first array `a[]` and the other at the first element of the second array `b[]`. The idea is to swap elements if they are out of order—i.e., if an element in `a[]` is larger than one in `b[]`. After the swapping process, both arrays will be individually sorted. Finally, we sort both arrays to achieve the final merged, sorted form without extra space.

### Steps:

1. Initialize two pointers:
   - `left` points to the last valid element of `a[]` (i.e., index `a.size()-1`).
   - `right` points to the first element of `b[]` (i.e., index `0`).
2. While moving the pointers:
   - If `a[left] > b[right]`, swap the elements and move the pointers.
   - If `a[left] <= b[right]`, the elements are in correct order, so stop.
3. Sort both `a[]` and `b[]` individually to maintain the order.
4. No extra array is used, and the result will be the merged arrays with all elements in sorted order.

### Time and Space Complexity

- **Time Complexity**: `O(n \log n + m \log m)`  
  Sorting both arrays dominates the time complexity.
- **Space Complexity**: `O(1)`  
  The solution modifies the input arrays in-place, without extra space.

### Code

```cpp
void mergeTwoSortedArraysWithoutExtraSpace(vector<long long> &a, vector<long long> &b) {
    int left = a.size() - 1, right = 0;

    // Swap elements if out of order
    while (left >= 0 && right < b.size()) {
        if (a[left] > b[right]) {
            swap(a[left], b[right]);
        }
        left--;
        right++;
    }

    // Sort both arrays individually
    sort(a.begin(), a.end());
    sort(b.begin(), b.end());
}
```

## Approach 3: Gap Method (Shell Sort Inspired)

### Idea:

This approach uses the gap method, inspired by Shell Sort. We treat both arrays as a single combined array and repeatedly compare elements that are at a gap distance apart. If they are out of order, we swap them. The gap is initially set to `(n + m) / 2`, and with each iteration, the gap is halved until it becomes zero. This method efficiently reduces the number of comparisons and swaps needed to merge the two arrays in-place.

### Steps:

1. Set the initial gap as `ceil((n + m) / 2)`, where `n` is the size of `a[]` and `m` is the size of `b[]`.
2. Use two pointers: `left` starts at 0, and `right` starts at `left + gap`.
3. Compare the elements at positions `left` and `right`. If they are out of order (i.e., `a[left] > a[right]` or `a[left] > b[right - n]` or `b[left - n] > b[right - n]`), swap them.
4. After finishing one pass with the current gap, reduce the gap to `ceil(gap / 2)` and repeat.
5. Stop when the gap becomes 0.

### Time and Space Complexity

- **Time Complexity**: `O((n + m) \log(n + m))`  
  Each pass reduces the gap by half, similar to Shell Sort, making the time complexity logarithmic.
- **Space Complexity**: `O(1)`  
  No extra space is used as the merging happens in place.

### Code

```cpp
void mergeTwoSortedArraysWithoutExtraSpace(vector<long long> &a, vector<long long> &b) {
    int n = a.size(), m = b.size();
    int gap = ceil((double)(n + m) / 2);

    while (gap > 0) {
        int left = 0, right = gap;

        while (right < (n + m)) {
            if (right < n && a[left] > a[right]) {
                // Compare and swap within array a
                swap(a[left], a[right]);
            } else if (left < n && right >= n && a[left] > b[right - n]) {
                // Compare and swap between arrays a and b
                swap(a[left], b[right - n]);
            } else if (left >= n && b[left - n] > b[right - n]) {
                // Compare and swap within array b
                swap(b[left - n], b[right - n]);
            }

            left++;
            right++;
        }

        // Reduce the gap for the next pass
        if (gap == 1) {
            break;
        }
        gap = ceil((double)gap / 2);
    }
}
```
