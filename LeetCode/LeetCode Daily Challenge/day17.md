# Maximum Swap

[Maximum Swap - LeetCode](https://leetcode.com/problems/maximum-swap/description)

## Approach 1: Brute Force

### Idea

1. Convert the given number to a string to easily access individual digits.
2. For each pair of digits `(i, j)` where `i < j`, swap them and check if the resulting number is greater than the current maximum.
3. After trying all possible swaps, return the maximum number found after converting the result back to an integer.

### Time Complexity

- **O(n^2)**: Where `n` is the number of digits in the input number. We try all pairs of swaps.

### Space Complexity

- **O(1)**: Only a few extra variables are used.

### Code

```cpp
int maximumSwap(int num) {
    string numStr = to_string(num);  // Convert number to string
    int maxNum = num;  // Initialize maxNum with the original number

    // Try all possible swaps
    for (int i = 0; i < numStr.size(); i++) {
        for (int j = i + 1; j < numStr.size(); j++) {
            // Swap digits at positions i and j
            swap(numStr[i], numStr[j]);

            // Update maxNum with the new integer value after swapping
            maxNum = max(maxNum, stoi(numStr));

            // Revert the swap to try other combinations
            swap(numStr[i], numStr[j]);
        }
    }

    return maxNum;  // Return the maximum number found
}
```

## Approach 2: Optimized Approach Using Last Occurrence Tracking

### Idea

1. Convert the given number to a string for easy digit access.
2. Store the **last occurrence of each digit (0-9)** using an array.
3. Traverse the number from left to right, and for each digit, check if a **larger digit appears later** in the number. If found, swap them to maximize the number.
4. Return the new number after converting it back to an integer.

### Time Complexity

- **O(n)**: We traverse the string twice — once to store the last occurrences and once to find the swap.

### Space Complexity

- **O(1)**: We only use a fixed-size array of size 10.

### Code

```cpp
int maximumSwap(int num) {
    string numStr = to_string(num);  // Convert number to string
    int n = numStr.size();

    // Step 1: Track the last occurrence of each digit (0-9)
    vector<int> last(10, -1);
    for (int i = 0; i < n; i++) {
        last[numStr[i] - '0'] = i;
    }

    // Step 2: Find the first place to swap for maximizing the number
    for (int i = 0; i < n; i++) {
        // Check if any larger digit exists after the current digit
        for (int d = 9; d > numStr[i] - '0'; d--) {
            if (last[d] > i) {  // Found a larger digit that appears later
                swap(numStr[i], numStr[last[d]]);  // Perform swap
                return stoi(numStr);  // Convert to int and return result
            }
        }
    }

    // If no swap is needed, return the original number
    return num;
}
```
