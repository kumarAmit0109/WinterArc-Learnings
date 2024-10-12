# 6. Rotate Array

### Problem Link

[LeetCode - Rotate Array](https://leetcode.com/problems/rotate-array/description/)

---

## Approach 1: Using a Temporary Array

**Idea**  
Step 1: Copy the last `k` elements into a temporary array.  
Step 2: Shift `n-k` elements from the beginning by `k` positions to the right.  
Step 3: Copy the elements from the temporary array into the main array.

**Time Complexity**  
Time Complexity: O(n)  
The function traverses the entire array of size `n` once, making the time complexity linear.

**Space Complexity**  
Space Complexity: O(k)  
We use extra space for the temporary array of size `k`.

**Code**

```cpp
void rotate(vector<int>& nums, int k) {
    int n = nums.size();
    if (n == 0) return; // Handle empty array case
    k = k % n; // Adjust k if it's greater than n

    vector<int> temp(k); // Temporary array to hold last k elements

    // Step 1: Copy the last k elements into the temp array
    for (int i = 0; i < k; i++) {
        temp[i] = nums[n - k + i];
    }

    // Step 2: Shift the first n-k elements to the right by k positions
    for (int i = n - 1; i >= k; i--) {
        nums[i] = nums[i - k];
    }

    // Step 3: Copy the elements from the temp array into the main array
    for (int i = 0; i < k; i++) {
        nums[i] = temp[i];
    }
}
```

## Approach 2: Using Modular Arithmetic

**Idea**  
Use modular arithmetic to find the new positions of each element directly.

**Time Complexity**  
Time Complexity: O(n)  
The function traverses the entire array of size `n` once, making the time complexity linear.

**Space Complexity**  
Space Complexity: O(n)  
We use a new array of size `n` to hold the rotated elements.

**Code**

```cpp
void rotate(vector<int>& nums, int k) {
    int n = nums.size();
    vector<int> arr(n);
    for (int i = 0; i < n; i++) {
        arr[(i + k) % n] = nums[i]; // Place each element in its new position
    }
    nums = arr; // Update the original array
}
```

## Approach 3: Reverse Method

**Idea**  
Step 1: Reverse the last `k` elements of the array.  
Step 2: Reverse the first `n-k` elements of the array.  
Step 3: Reverse the entire array.

**Time Complexity**  
Time Complexity: O(n)  
The function traverses the entire array of size `n` three times, resulting in linear time complexity.

**Space Complexity**  
Space Complexity: O(1)  
This approach uses constant extra space.

**Code**

```cpp
void reverse(vector<int>& nums, int left, int right) {
    while (left < right) {
        swap(nums[left], nums[right]);
        left++;
        right--;
    }
}

void rotate(vector<int>& nums, int k) {
    int n = nums.size();
    k = k % n; // Handle cases where k >= n

    // Step 1: Reverse the last k elements
    reverse(nums, n - k, n - 1);
    // Step 2: Reverse the first n-k elements
    reverse(nums, 0, n - k - 1);
    // Step 3: Reverse the whole array
    reverse(nums, 0, n - 1);
}
```

# 7. Move Zeroes

### Problem Link

[LeetCode - Move Zeroes](https://leetcode.com/problems/move-zeroes/description/)

---

## Approach 1: Using Two Pointers

**Idea**  
We maintain a pointer `last_non_zero_found_at` to track the position of the last non-zero element. As we traverse the array, whenever we encounter a non-zero element, we swap it with the element at `last_non_zero_found_at`.

**Time Complexity**  
Time Complexity: O(n)  
We traverse the array once, where `n` is the size of the array.

**Space Complexity**  
Space Complexity: O(1)  
We only use a constant amount of extra space for the pointer `last_non_zero_found_at`.

**Code**

```cpp
void moveZeroes(vector<int>& nums) {
    int last_non_zero_found_at = 0; // Pointer for the position of the last non-zero element

    // Move non-zero elements forward
    for (int current = 0; current < nums.size(); current++) {
        if (nums[current] != 0) {
            // Swap the current non-zero element with the element at last_non_zero_found_at
            swap(nums[last_non_zero_found_at], nums[current]);
            last_non_zero_found_at++; // Increment the position for the next non-zero
        }
    }
}
```

## Approach 2: Using a Single Pointer

**Idea**  
This approach uses a single pointer `j` to keep track of the position for the next non-zero element while iterating through the array.

**Time Complexity**  
Time Complexity: O(n)  
We traverse the array once, where `n` is the size of the array.

**Space Complexity**  
Space Complexity: O(1)  
We only use a constant amount of extra space for the pointer `j`.

**Code**

```cpp
void moveZeroes(vector<int>& nums) {
    int j = 0; // Pointer to track the position of non-zero elements
    for (int i = 0; i < nums.size(); i++) {
        if (nums[i] != 0) {
            swap(nums[i], nums[j]); // Swap non-zero element with the position at j
            j++; // Increment the position for the next non-zero
        }
    }
}
```

# 8. Union of Two Sorted Arrays

### Problem Link

[GeeksforGeeks - Union of Two Sorted Arrays](https://www.geeksforgeeks.org/problems/union-of-two-sorted-arrays-1587115621/1)

---

## Approach 1: Using Set/Map

**Idea**  
We can use a set or map to store the unique elements from both arrays. Since sets do not allow duplicates, all the distinct elements from both arrays will be stored in the set. After traversing both arrays, we can return the unique elements from the set in the correct order.

**Time Complexity**  
Time Complexity: O(n + m)  
We traverse both arrays, where `n` is the size of the first array and `m` is the size of the second array. The insertion and lookup operations in a set/map are O(1) on average.

**Space Complexity**  
Space Complexity: O(n + m)  
We use extra space for the set/map to store the unique elements from both arrays.

**Code**

```cpp
vector<int> findUnion(int arr1[], int arr2[], int n, int m) {
    set<int> unionSet; // Using a set to store unique elements

    // Insert elements from the first array
    for (int i = 0; i < n; i++) {
        unionSet.insert(arr1[i]);
    }

    // Insert elements from the second array
    for (int i = 0; i < m; i++) {
        unionSet.insert(arr2[i]);
    }

    // Convert set to vector for result
    vector<int> result(unionSet.begin(), unionSet.end());
    return result;
}
```

## Approach 2: Two-Pointer Technique

### Idea

We use two pointers, `i` and `j`, to traverse the two sorted arrays simultaneously:

1. **If `arr1[i] == arr2[j]`**:  
   We add the element to the result and increment both pointers.

2. **If `arr1[i] > arr2[j]`**:  
   We add `arr2[j]` to the result and increment `j`.

3. **If `arr1[i] < arr2[j]`**:  
   We add `arr1[i]` to the result and increment `i`.

After traversing both arrays, if any elements are left in either array, we add them to the result.

### Time Complexity

**Time Complexity**: O(n + m)  
We traverse both arrays once using two pointers, where `n` is the size of the first array and `m` is the size of the second array.

### Space Complexity

**Space Complexity**: O(n + m)  
We use extra space for the result array to store the union of both arrays.

### Code

```cpp
vector<int> findUnion(int arr1[], int arr2[], int n, int m) {
    vector<int> unionArr; // Resultant union array
    int i = 0, j = 0;

    while (i < n && j < m) {
        // Case 1: Both elements are equal
        if (arr1[i] == arr2[j]) {
            // Insert only if it's not a duplicate
            if (unionArr.empty() || unionArr.back() != arr1[i]) {
                unionArr.push_back(arr1[i]);
            }
            i++;
            j++;
        }
        // Case 2: arr1[i] > arr2[j]
        else if (arr1[i] > arr2[j]) {
            if (unionArr.empty() || unionArr.back() != arr2[j]) {
                unionArr.push_back(arr2[j]);
            }
            j++;
        }
        // Case 3: arr1[i] < arr2[j]
        else {
            if (unionArr.empty() || unionArr.back() != arr1[i]) {
                unionArr.push_back(arr1[i]);
            }
            i++;
        }
    }

    // Add remaining elements from arr1
    while (i < n) {
        if (unionArr.empty() || unionArr.back() != arr1[i]) {
            unionArr.push_back(arr1[i]);
        }
        i++;
    }

    // Add remaining elements from arr2
    while (j < m) {
        if (unionArr.empty() || unionArr.back() != arr2[j]) {
            unionArr.push_back(arr2[j]);
        }
        j++;
    }

    return unionArr;
}
```

# 9. Missing Number

### Problem Link

[LeetCode - Missing Number](https://leetcode.com/problems/missing-number/description/)

---

## Approach 1: Frequency Array

### Idea

We create a frequency array `freqArr` of size `n+1` where `n` is the length of the given array. We traverse the array and update the frequency of each number from `0` to `n`. After traversing, the index in `freqArr` with a value of `0` will be the missing number.

### Time Complexity

**Time Complexity**: O(n)  
We traverse the array once to update the frequency array and then traverse the frequency array to find the missing number.

### Space Complexity

**Space Complexity**: O(n)  
We use an extra frequency array of size `n+1`.

### Code

```cpp
class Solution {
public:
    int missingNumber(vector<int>& nums) {
        int n = nums.size();
        vector<int> freqArr(n+1, 0); // Frequency array initialized with 0

        // Traverse nums and mark the frequency in freqArr
        for (int i = 0; i < n; i++) {
            freqArr[nums[i]]++;
        }

        // Find the index with frequency 0, which is the missing number
        for (int i = 0; i <= n; i++) {
            if (freqArr[i] == 0) {
                return i;
            }
        }

        return -1; // In case no missing number is found, though it shouldn't happen
    }
};
```

## Approach 2: Sum Formula

### Idea

We use the sum formula for the first `n` natural numbers:

\[
\text{Sum} = \frac{n(n+1)}{2}
\]

We compute this sum and subtract the sum of all elements in the given array. The remaining difference will be the missing number.

### Time Complexity

**Time Complexity**: O(n)  
We traverse the array once to calculate the sum and subtract the array elements from it.

### Space Complexity

**Space Complexity**: O(1)  
We use constant space, only storing a few variables to compute the sum.

### Code

```cpp
class Solution {
public:
    int missingNumber(vector<int>& nums) {
        int sum = 0;

        // Step 1: Calculate the sum of numbers from 0 to n
        for (int i = 0; i < nums.size() + 1; i++) {
            sum = sum + i;
        }

        // Step 2: Subtract each element in the array from the sum
        for (int i = 0; i < nums.size(); i++) {
            sum = sum - nums[i];
        }

        // Return the missing number (remaining sum)
        return sum;
    }
};
```

## Approach 3: XOR Method

### Idea

We can use the XOR operator to find the missing number. The key property of XOR is that `x ^ x = 0` and `x ^ 0 = x`. Using this property, we can XOR all the numbers from `0` to `n` with all the numbers in the array. The result will be the missing number because all other numbers cancel out.

### Steps:

1. XOR all numbers from `0` to `n`.
2. XOR the result with all elements in the array.
3. The remaining result after all XOR operations will be the missing number.

### Time Complexity

**Time Complexity**: O(n)  
We traverse the array once, applying XOR operations.

### Space Complexity

**Space Complexity**: O(1)  
No extra space is used apart from the variables for XOR operations.

### Code

```cpp
class Solution {
public:
    int missingNumber(vector<int>& nums) {
        int xorSum = 0;

        // Step 1: XOR all numbers from 0 to n
        for (int i = 0; i < nums.size(); i++) {
            xorSum = xorSum ^ i ^ nums[i];
        }

        // Step 2: XOR with n (since the loop goes only till n-1)
        return xorSum ^ nums.size();
    }
};
```

# 10. Max Consecutive Ones

### Problem Link

[LeetCode - Max Consecutive Ones](https://leetcode.com/problems/max-consecutive-ones/description/)

---

## Approach: Count Consecutive Ones

### Idea

We can iterate through the binary array and keep a count of consecutive `1`s. Whenever we encounter a `0`, we check if the current count of `1`s is greater than the maximum found so far and update accordingly. We then reset the count and continue traversing the array.

### Steps:

1. Initialize `maxi` to keep track of the maximum number of consecutive `1`s found and `count` to count the current consecutive `1`s.
2. Traverse through the array.
3. If the current element is `1`, increment the `count`.
4. If the current element is `0`, update `maxi` if `count` is greater than `maxi`, then reset `count` to `0`.
5. After the loop, check once more to update `maxi` with the last count if it was the maximum.

### Time Complexity

**Time Complexity**: O(n)  
We traverse the array once.

### Space Complexity

**Space Complexity**: O(1)  
No extra space is used apart from a few variables.

### Code

```cpp
int findMaxConsecutiveOnes(vector<int>& nums) {
    int maxi = 0, count = 0;
    for (int i = 0; i < nums.size(); i++) {
        if (nums[i] == 1) {
            count++;
            continue;
        }
        maxi = max(maxi, count);
        count = 0;
    }
    return max(maxi, count);
}
```
