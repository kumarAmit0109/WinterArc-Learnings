# 36. Missing and Repeating

### Problem Link

[Leetcode - Missing and Repeating](https://www.naukri.com/code360/problems/missing-and-repeating_8165381)

## Approach 1: Hashing

1. **Idea**: Use a hash map to track the frequency of each number. This allows us to identify the missing and repeating numbers efficiently.

2. **Steps**:

   - Create a hash map (or an array) to count occurrences of each number from 1 to n.
   - Traverse the array and increment the count for each number.
   - In a second pass, check for the number that has a count of 2 (repeating) and the number that has a count of 0 (missing).

3. **Time Complexity**: `O(n)`  
   We traverse the array twice, once to count and once to find the missing and repeating numbers.

4. **Space Complexity**: `O(n)`  
   We use extra space for the hash map (or array).

### Code

```cpp
pair<int, int> findMissingAndRepeating(vector<int> arr, int n) {
    unordered_map<int, int> freqMap;
    int missing = -1, repeating = -1;

    // Step 1: Count frequencies
    for (int num : arr) {
        freqMap[num]++;
    }

    // Step 2: Find missing and repeating
    for (int i = 1; i <= n; i++) {
        if (freqMap[i] == 0) {
            missing = i;  // This number is missing
        } else if (freqMap[i] == 2) {
            repeating = i;  // This number is repeating
        }
    }

    return {missing, repeating};
}
```

## Approach 2: Mathematical Approach

1. **Idea**: Use the properties of sum and square of numbers to find the missing and repeating numbers.

2. **Steps**:

   - Calculate the expected sum and expected sum of squares for numbers from 1 to n.
   - Compute the actual sum and actual sum of squares from the array.
   - Use the differences between expected and actual sums to derive the missing and repeating numbers.

3. **Time Complexity**: `O(n)`  
   We traverse the array once to calculate the sums.

4. **Space Complexity**: `O(1)`  
   We use only a few extra variables for calculations.

### Code

```cpp
pair<int, int> findMissingAndRepeating(vector<int> arr, int n) {
    int sumN = n * (n + 1) / 2; // Expected sum
    int sumSquareN = n * (n + 1) * (2 * n + 1) / 6; // Expected sum of squares

    int actualSum = 0, actualSumSquare = 0;

    for (int num : arr) {
        actualSum += num;
        actualSumSquare += num * num;
    }

    int sumDiff = sumN - actualSum; // Missing - Repeating
    int squareDiff = sumSquareN - actualSumSquare; // Missing^2 - Repeating^2

    // (missing + repeating) = (squareDiff / sumDiff)
    int missingPlusRepeating = squareDiff / sumDiff;

    int missing = (missingPlusRepeating + sumDiff) / 2; // (missing + repeating + missing - repeating) / 2
    int repeating = missingPlusRepeating - missing; // missing + repeating - missing

    return {missing, repeating};
}
```

## Approach 3: Optimal Approach 2 (Using XOR)

1. **Idea**: Utilize XOR properties to identify the missing and repeating numbers efficiently.

2. **Steps**:

   - Assume the repeating number is \(X\) and the missing number is \(Y\).
   - **Step 1**: Calculate \(X \oplus Y\) using the formula:
     \[
     X \oplus Y = (a[0] \oplus a[1] \oplus ... \oplus a[n-1]) \oplus (1 \oplus 2 \oplus ... \oplus N)
     \]
   - **Step 2**: Identify the first differing bit from the right in \(X \oplus Y\).
   - **Step 3**: Group all elements (array elements + numbers from 1 to N) into two groups based on the identified bit.
   - **Last Step**: Determine which number is repeating and which is missing by checking the occurrences in the original array.

3. **Time Complexity**: `O(n)`  
   The algorithm requires only linear time to traverse the array and perform calculations.

4. **Space Complexity**: `O(1)`  
   The approach uses a constant amount of extra space.

### Code

```cpp
vector<int> findTwoElement(vector<int>& arr) {
    int n = arr.size();
    int xr = 0;

    // Step 1: Find XOR of all elements:
    for (int i = 0; i < n; i++) {
        xr ^= arr[i];           // XOR of array elements
        xr ^= (i + 1);         // XOR with numbers from 1 to N
    }

    // Step 2: Find the differentiating bit number:
    int number = (xr & ~(xr - 1));

    // Step 3: Group the numbers:
    int zero = 0;
    int one = 0;

    for (int i = 0; i < n; i++) {
        // Part of 1 group:
        if ((arr[i] & number) != 0) {
            one ^= arr[i];
        }
        // Part of 0 group:
        else {
            zero ^= arr[i];
        }
    }

    for (int i = 1; i <= n; i++) {
        // Part of 1 group:
        if ((i & number) != 0) {
            one ^= i;
        }
        // Part of 0 group:
        else {
            zero ^= i;
        }
    }

    // Last step: Identify the numbers:
    int cnt = 0;
    for (int i = 0; i < n; i++) {
        if (arr[i] == zero) cnt++;
    }

    if (cnt == 2) return {zero, one}; // zero is repeating
    return {one, zero}; // one is repeating
}
```

# 37. Number of Inversions

### Problem Link

[Naukri - Number of Inversions](https://www.naukri.com/code360/problems/number-of-inversions_6840276)

## Approach 1: Naive (Brute Force)

### Idea

Count the total number of inversions by checking every possible pair `(a[i], a[j])` where `i < j` and `a[i] > a[j]`. This approach uses two nested loops to find all such pairs.

### Steps:

1. **Outer Loop**: Iterate through each element `a[i]` from index `0` to `n-1`.
2. **Inner Loop**: For each `a[i]`, check all elements `a[j]` with `j > i` and see if `a[i] > a[j]`.
3. **Count**: If the condition is satisfied, increment the count of inversions.

### Time Complexity

- `O(n^2)` because of two nested loops.

### Space Complexity

- `O(1)` since no extra space is used, except for a few variables.

### Code

```cpp
int numberOfInversions(vector<int>& a, int n) {
    int count = 0;
    // Outer loop to pick the first element in the pair
    for (int i = 0; i < n; i++) {
        // Inner loop to pick the second element in the pair
        for (int j = i + 1; j < n; j++) {
            // Check if a[i] > a[j]
            if (a[i] > a[j]) {
                count++;
            }
        }
    }
    return count;  // Return the total count of inversions
}
```

## Approach 2: Optimal (Merge Sort)

### Idea

The merge sort algorithm can be modified to count inversions efficiently while sorting the array. During the merging step, if an element from the right half is smaller than an element from the left half, then all subsequent elements in the left half will also form inversions with this element (since the array is sorted in both halves).

### Steps:

1. **Divide** the array into two halves recursively as in the merge sort.
2. **Count inversions** during the merging step:
   - If `left[i] > right[j]`, then all elements after `left[i]` will also be greater than `right[j]` (because both halves are sorted).
   - Add the number of such pairs to the inversion count.
3. **Merge the two halves** back into a sorted array.

### Time Complexity

- `O(n log n)` due to the merge sort algorithm, where `n` is the size of the array.

### Space Complexity

- `O(n)` for the temporary array used during merging.

---

### Code

```cpp
int mergeAndCount(vector<int>& a, int low, int mid, int high) {
    int cnt = 0;  // Count of inversions
    vector<int> temp;  // Temporary array for merged result

    int left = low;  // Left pointer (start of first half)
    int right = mid + 1;  // Right pointer (start of second half)

    // Merge two halves and count inversions
    while (left <= mid && right <= high) {
        if (a[left] <= a[right]) {
            temp.push_back(a[left]);  // No inversion
            left++;
        } else {
            temp.push_back(a[right]);
            cnt += (mid - left + 1);  // All elements after left form an inversion
            right++;
        }
    }

    // Copy remaining elements from the left half
    while (left <= mid) {
        temp.push_back(a[left]);
        left++;
    }

    // Copy remaining elements from the right half
    while (right <= high) {
        temp.push_back(a[right]);
        right++;
    }

    // Copy sorted elements back into the original array
    for (int i = low; i <= high; i++) {
        a[i] = temp[i - low];
    }

    return cnt;
}

int mergeSortAndCount(vector<int>& a, int low, int high) {
    int cnt = 0;
    if (low < high) {
        int mid = low + (high - low) / 2;

        // Count inversions in the left half
        cnt += mergeSortAndCount(a, low, mid);

        // Count inversions in the right half
        cnt += mergeSortAndCount(a, mid + 1, high);

        // Count inversions while merging
        cnt += mergeAndCount(a, low, mid, high);
    }
    return cnt;
}

int numberOfInversions(vector<int>& a, int n) {
    return mergeSortAndCount(a, 0, n - 1);  // Call merge sort and count inversions
}
```

# 40. Binary Search

### Problem Link

[LeetCode - Binary Search ](https://leetcode.com/problems/binary-search/description/)

## Approach: Iterative Binary Search

### Idea

The binary search algorithm is an efficient way to search for a target element in a sorted array. The idea is to repeatedly divide the array into halves and compare the target with the middle element. Depending on whether the target is smaller or larger than the middle element, we discard one half of the array.

### Steps:

1. **Initialize Pointers**:
   - `start` points to the beginning of the array.
   - `end` points to the last element of the array.
2. **Binary Search**:
   - While `start <= end`, calculate the middle index `mid` as `start + (end - start) / 2` (to avoid overflow).
   - Compare the element at `mid` with the target:
     - If `nums[mid] == target`, return `mid`.
     - If `nums[mid] > target`, the target is in the left half, so move `end` to `mid - 1`.
     - If `nums[mid] < target`, the target is in the right half, so move `start` to `mid + 1`.
3. **Return -1**: If the target is not found, return `-1`.

### Time Complexity

- `O(log n)` because the array is divided into halves in each iteration.

### Space Complexity

- `O(1)` since no additional space is required.

---

### Code

```cpp
int search(vector<int>& nums, int target) {
    int start = 0;
    int end = nums.size() - 1;

    // Binary search loop
    while (start <= end) {
        int mid = start + (end - start) / 2;  // Calculate mid to avoid overflow

        // Check if the target is found
        if (nums[mid] == target) {
            return mid;  // Target found at index mid
        }

        // Adjust search space
        if (nums[mid] > target) {
            end = mid - 1;  // Search in the left half
        } else {
            start = mid + 1;  // Search in the right half
        }
    }

    return -1;  // Target not found
}
```
