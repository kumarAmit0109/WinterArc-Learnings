# 1. Largest Element in an Array

### Problem Link

[GeeksforGeeks - Largest Element in an Array](https://www.geeksforgeeks.org/problems/largest-element-in-array4009/0)

### Approach

1. **Initialize** a variable `maxElement` to `INT_MIN`, which represents the smallest possible integer in C++.
2. **Traverse** through the array using a for-loop:
   - Compare each element with `maxElement`.
   - If the current element is greater than `maxElement`, update `maxElement` with that value.
3. **Return** `maxElement` once the loop completes. This will be the largest element in the array.

### Time and Space Complexity

- **Time Complexity**: `O(n)`  
  The function traverses the entire array of size `n` exactly once, making the time complexity linear.
- **Space Complexity**: `O(1)`  
  The function uses a constant amount of extra space for variables like `maxElement`, regardless of the size of the input array.

### Code

```cpp
int largest(vector<int> &arr) {
    int maxElement = INT_MIN; // Initialize maxElement with the smallest possible integer

    // Traverse the array to find the maximum element
    for (int i = 0; i < arr.size(); i++) {
        if (arr[i] > maxElement) {
            maxElement = arr[i]; // Update maxElement if current element is greater
        }
    }

    return maxElement;
}
```

# 2. Second Largest Element in an Array

### Problem Link

[GeeksforGeeks - Second Largest Element in an Array](https://www.geeksforgeeks.org/problems/second-largest3735/1)

## Approach 1: Sorting Method

1. **Idea**: If it is given that all elements in the array are unique, we can sort the array and return the second last element.
2. **Time Complexity**: `O(n log n)` (due to sorting)
3. **Space Complexity**: `O(1)` (if sorting is done in-place)

### Code

```cpp
int getSecondLargest(vector<int> &arr) {
    sort(arr.begin(), arr.end()); // Sort the array
    return arr[arr.size() - 2];    // Return the second last element
}
```

## Approach 2: Two Traversals by Replacing Largest

### Idea

Traverse the array twice. In the first traversal, find the largest element and replace it with a value that is not in the array (e.g., `INT_MIN` for positive arrays). In the second traversal, find the largest element again, which will now be the second largest.

### Time Complexity

`O(2n)` = `O(n)` (since there are two traversals of the array)

### Space Complexity

`O(1)` (constant extra space)

### Code

```cpp
int getSecondLargest(vector<int> &arr) {
    int largest = INT_MIN;

    // First traversal to find the largest element
    for (int i = 0; i < arr.size(); i++) {
        if (arr[i] > largest) {
            largest = arr[i];
        }
    }

    // Replace the largest element with INT_MIN
    for (int i = 0; i < arr.size(); i++) {
        if (arr[i] == largest) {
            arr[i] = INT_MIN;
            break;
        }
    }

    // Second traversal to find the second largest element
    int secondLargest = INT_MIN;
    for (int i = 0; i < arr.size(); i++) {
        if (arr[i] > secondLargest) {
            secondLargest = arr[i];
        }
    }

    return secondLargest;
}
```

## Approach 3: Two Traversals without Replacement

### Idea

Traverse the array twice. In the first traversal, find the largest element. In the second traversal, find the element which is just smaller than the largest element (i.e., the second largest).

### Time Complexity

`O(2n)` = `O(n)` (since there are two traversals of the array)

### Space Complexity

`O(1)` (constant extra space)

### Code

```cpp
int getSecondLargest(vector<int> &arr) {
    int largest = INT_MIN;

    // First traversal to find the largest element
    for (int i = 0; i < arr.size(); i++) {
        if (arr[i] > largest) {
            largest = arr[i];
        }
    }

    // Second traversal to find the second largest element
    int secondLargest = INT_MIN;
    for (int i = 0; i < arr.size(); i++) {
        if (arr[i] > secondLargest && arr[i] < largest) {
            secondLargest = arr[i];
        }
    }

    return secondLargest;
}
```

## Approach 4: Single Traversal (Optimal)

### Idea

Traverse the array once. While traversing, if an element is greater than the current largest, update `secondLargest` to the previous largest, and then update `largest` to the new element. Additionally, if the current element is greater than `secondLargest` but smaller than `largest`, update `secondLargest`.

### Time Complexity

`O(n)` (single traversal of the array)

### Space Complexity

`O(1)` (constant extra space)

### Code

```cpp
int getSecondLargest(vector<int> &arr) {
    int largest = INT_MIN, secondLargest = INT_MIN;

    for (int i = 0; i < arr.size(); i++) {
        if (arr[i] > largest) {
            secondLargest = largest;
            largest = arr[i];
        } else if (arr[i] > secondLargest && arr[i] < largest) {
            secondLargest = arr[i];
        }
    }

    return (secondLargest == INT_MIN) ? -1 : secondLargest;
}
```

### Reference

For more details, check
[this article](https://medium.com/@gokulvaradan/find-second-largest-and-second-smallest-element-in-the-array-11c169e902b4)

# 3. Check if an Array is Sorted

### Problem Link

[GeeksforGeeks - Check if an Array is Sorted](https://www.geeksforgeeks.org/problems/check-if-an-array-is-sorted0701/1)

## Approach 1: Recursive Method

### Idea

1. If the size of the array is zero or one, return true.
2. Check the last two elements of the array. If they are sorted, perform a recursive call with `n-1`; otherwise, return false.
3. If all elements are found sorted, `n` will eventually fall to one, satisfying Step 1.

### Time Complexity

`O(n)` (where `n` is the size of the array)

### Space Complexity

`O(n)` (due to the recursion stack)

### Code

```cpp
bool arraySortedOrNot(vector<int>& arr) {
    int n = arr.size();
    if (n <= 1) return true;

    if (arr[n - 1] < arr[n - 2]) {
        return false;
    }

    vector<int> subArray(arr.begin(), arr.end() - 1);
    return arraySortedOrNot(subArray);
}
```

## Approach 2: Iterative Method

### Idea

The iterative approach avoids the usage of recursion stack space and recursion overhead. The logic is to iterate over the whole array and, in every iteration, check whether `arr[i] > arr[i-1]` or not.

### Time Complexity

`O(n)` (where `n` is the size of the array)

### Space Complexity

`O(1)` (constant extra space)

### Code

```cpp
bool arraySortedOrNot(vector<int>& arr) {
    for (int i = 1; i < arr.size(); i++) {
        if (arr[i] < arr[i - 1]) {
            return false;
        }
    }
    return true;
}
```

# 4. Remove Duplicates from Sorted Array

### Problem Link

[LeetCode - Remove Duplicates from Sorted Array](https://leetcode.com/problems/remove-duplicates-from-sorted-array/)

## Approach 1: Using Hashmap/Set

### Idea

We can use a `std::unordered_set` to track unique elements in the array. As we traverse the array, we insert each element into the set. For every unique element found, place it in the first `k` positions of the original array.

### Time Complexity

- **Time Complexity**: `O(n)`  
  We traverse the array once, and the set insertion and lookup operations are `O(1)` on average, where `n` is the size of the array.

### Space Complexity

- **Space Complexity**: `O(k)`  
  We use extra space for storing the unique elements in the set, where `k` is the number of unique elements.

### Code

```cpp
int removeDuplicates(std::vector<int>& nums) {
    std::unordered_set<int> uniqueElements;
    int k = 0; // Index for placing unique elements

    for (int num : nums) {
        // If the number is not in the set, it's unique
        if (uniqueElements.insert(num).second) {
            nums[k++] = num; // Place unique element in the array
        }
    }

    return k; // Return the new length of the array
}
```

## Approach 2: Two Pointers Technique

### Idea

We use two pointers: `i` and `j`. Both start at the beginning of the array. The pointer `j` traverses the array while `i` tracks the position of unique elements. When we find that `nums[j] != nums[i]`, we increment `i` and assign `nums[i] = nums[j]`.

### Time Complexity

- **Time Complexity**: `O(n)`  
  The array is traversed once using pointer `j`, where `n` is the size of the array.

### Space Complexity

- **Space Complexity**: `O(1)`  
  We only use constant extra space for the two pointers.

### Code

```cpp
int removeDuplicates(vector<int>& nums) {
    if (nums.size() == 0) return 0;

    int i = 0; // Pointer to track unique elements

    for (int j = 1; j < nums.size(); j++) {
        // If the current element is different from the last unique one
        if (nums[j] != nums[i]) {
            i++; // Move to the next position for a unique element
            nums[i] = nums[j]; // Update the position with the new unique element
        }
    }

    return i + 1; // The length of the array with unique elements
}
```

# 5. Left Rotate an Array by One

### Problem Link

[Naukri - Left Rotate an Array by One](https://www.naukri.com/code360/problems/left-rotate-an-array-by-one_5026278)

### Approach 1: Using a Dummy Array

**Idea**  
We can take another dummy array of the same length and then shift all elements in the array toward the left. Finally, store the first element at the last position of the dummy array and return it.

**Time Complexity**  
Time Complexity: O(n)  
The function traverses the entire array of size `n` once, making the time complexity linear.

**Space Complexity**  
Space Complexity: O(n)  
We use extra space for the dummy array.

**Code**

```cpp
#include <vector>
using namespace std;

vector<int> rotateArray(vector<int>& arr, int n) {
    // Approach 1: Using a dummy array
    vector<int> rotatedArray(n); // Create a dummy array of the same length
    for (int i = 0; i < n - 1; i++) {
        rotatedArray[i] = arr[i + 1]; // Shift all elements towards the left
    }
    rotatedArray[n - 1] = arr[0]; // Store the first element at the end
    return rotatedArray; // Return the rotated array
}
```

### Approach 2: Using a Temp Variable

**Idea**  
Store the first element in a temporary variable. Shift every element one position back, then assign the first value to the last position.

**Time Complexity**  
Time Complexity: O(n)  
The function traverses the entire array of size `n` once, making the time complexity linear.

**Space Complexity**  
Space Complexity: O(1)  
We only use constant extra space for the temp variable.

**Code**

```cpp
vector<int> rotateArray(vector<int>& arr, int n) {
    // Approach 2: Using a temp variable
    if (n == 0) return arr; // Handle empty array case

    int temp = arr[0]; // Store the first element in temp
    for (int i = 0; i < n - 1; i++) {
        arr[i] = arr[i + 1]; // Shift every element one position behind
    }
    arr[n - 1] = temp; // Assign the first value = last value (stored in temp variable)
    return arr; // Return the rotated array
}
```
