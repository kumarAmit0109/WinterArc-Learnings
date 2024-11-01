# 146. Set the Rightmost Unset Bit

### Problem Link

[GeeksforGeeks - Set the Rightmost Unset Bit](https://www.geeksforgeeks.org/problems/set-the-rightmost-unset-bit4436/1)

## Approach 1: Convert to Binary and Set the Rightmost Unset Bit

### Algorithm

1. Convert the integer `n` to its binary representation.
2. Traverse the binary representation from the rightmost bit.
3. Find the first unset (0) bit.
4. Set this bit to 1 and return the new number.

### Time Complexity

- **O(log n)**: The time complexity depends on the number of bits in `n`.

### Space Complexity

- **O(1)**: No extra space is used.

### Code

```cpp
int setBit(int n) {
    int pos = 0;
    while ((n & (1 << pos)) != 0) {
        pos++;
    }
    return n | (1 << pos);
}
```

## Approach 2: Bit Manipulation (Using `n | (n + 1)`)

### Algorithm

1. Use the formula `n | (n + 1)` to set the rightmost unset bit.
   - Adding `1` to `n` flips the rightmost unset bit and all bits to the right.
   - Performing `n | (n + 1)` sets the rightmost unset bit in `n`.
2. Return the result of `n | (n + 1)`.

### Time Complexity

- **O(1)**: This approach uses a single bitwise operation.

### Space Complexity

- **O(1)**: No extra space is used.

### Code

```cpp
int setBit(int n) {
    return n | (n + 1);
}
```

# 147. Swap Two Numbers

### Problem Link

[Naukri - Swap Two Numbers](https://www.naukri.com/code360/problems/swap-two-numbers_1112577)

## Approach 1: Using a Temporary Variable

### Algorithm

1. Store the value of `a` in a temporary variable `temp`.
2. Assign the value of `b` to `a`.
3. Assign the value of `temp` (original `a`) to `b`.

### Time Complexity

- **O(1)**: Constant time operations.

### Space Complexity

- **O(1)**: Constant space is used.

### Code

```cpp
void swapNumber(int &a, int &b) {
    int temp = a;
    a = b;
    b = temp;
}
```

## Approach 2: Using Addition and Subtraction

### Algorithm

1. Add `b` to `a` and store the result in `a`.
2. Subtract the new `a` (sum) by `b` and assign the result to `b`.
3. Subtract the new `a` (sum) by the new `b` to get the original value of `b` and assign it to `a`.

### Time Complexity

- **O(1)**: Constant time operations.

### Space Complexity

- **O(1)**: Constant space is used.

### Code

```cpp
void swapNumber(int &a, int &b) {
    a = a + b;
    b = a - b;
    a = a - b;
}
```

## Approach 3: Using XOR

### Algorithm

1. Use XOR operations to swap values without a temporary variable.
   - `a = a ^ b` sets `a` to `a XOR b`.
   - `b = a ^ b` sets `b` to `a XOR b XOR b`, which simplifies to `a`.
   - `a = a ^ b` sets `a` to `a XOR b XOR a`, which simplifies to `b`.

### Time Complexity

- **O(1)**: Constant time operations.

### Space Complexity

- **O(1)**: Constant space is used.

### Code

```cpp
void swapNumber(int &a, int &b) {
    a = a ^ b;
    b = a ^ b;
    a = a ^ b;
}
```

# 148. Divide Two Integers

### Problem Link

[LeetCode - Divide Two Integers](https://leetcode.com/problems/divide-two-integers/description/)

## Approach 1: Repeated Subtraction

### Algorithm

1. Initialize `count` as `0`.
2. Keep adding `divisor` to `sum` until `sum + divisor` exceeds `dividend`.
3. Each time, increment `count`.
4. Return `count` as the quotient with the correct sign adjustment.

### Time Complexity

- **O(n)**: Proportional to `dividend` divided by `divisor`.

### Space Complexity

- **O(1)**: Constant space usage.

### Code

```cpp
int divide(int dividend, int divisor) {
    if (dividend == divisor) {
        return 1;
    }

    bool sign = (dividend >= 0) == (divisor > 0);
    long n = abs(dividend), d = abs(divisor);
    long quotient = 0;

    while (n >= d) {
        n -= d;
        quotient++;
    }

    if (quotient > INT_MAX) {
        return sign ? INT_MAX : INT_MIN;
    }

    return sign ? quotient : -quotient;
}

```

## Approach 2: Bit Manipulation (Optimized Shift-Based Subtraction)

### Algorithm

1. If `dividend` is `INT_MIN` (`-2147483648`) and `divisor` is `-1`, return `INT_MAX` to prevent overflow.
2. Determine the sign of the result based on the signs of `dividend` and `divisor`.
3. Take the absolute values of `dividend` and `divisor`.
4. Initialize `quotient` to `0`.
5. Use a loop to repeatedly subtract the largest possible shifted value of `divisor` from `dividend`:
   - Initialize `cnt` to `0`.
   - Left shift `divisor` until it is just smaller than `dividend`.
   - Subtract this shifted value from `dividend` and add `1 << cnt` to `quotient`.
6. Return `quotient` with the appropriate sign.

### Time Complexity

- **O(log n)**: Each division operation halves the problem size due to bit shifts.

### Space Complexity

- **O(1)**: Constant space usage.

### Code

```cpp
int divide(int dividend, int divisor) {
    // Step 1: Handle overflow case where dividing INT_MIN by -1 would exceed the int range
    if (dividend == INT_MIN && divisor == -1) {
        return INT_MAX;
    }

    // Step 2: Determine the sign of the result
    bool sign = true;
    if ((dividend >= 0 && divisor < 0) || (dividend < 0 && divisor > 0)) {
        sign = false;
    }

    // Step 3: Take the absolute values of dividend and divisor for calculation
    long n = abs(dividend);
    long d = abs(divisor);
    long quotient = 0;

    // Step 4: Perform division using bitwise operations
    while (n >= d) {
        int cnt = 0;

        // Step 5: Find the largest multiple of divisor (shifted left) that fits into dividend
        while (n >= (d << (cnt + 1))) {
            cnt++;
        }

        // Step 6: Add this multiple to the quotient
        quotient += 1 << cnt;

        // Step 7: Subtract the current multiple of divisor from dividend
        n -= d << cnt;
    }

    // Step 8: Apply the sign to the result
    return sign ? quotient : -quotient;
}
```

# 149. Minimum Bit Flips to Convert Number

### Problem Link

[LeetCode - Minimum Bit Flips to Convert Number](https://leetcode.com/problems/minimum-bit-flips-to-convert-number/)

## Approach 1: Bitwise Comparison

### Algorithm

1. Initialize `count` as `0`.
2. Use a loop to extract the least significant bits (LSBs) of `start` and `goal` until both numbers are zero.
3. Compare the LSBs:
   - If they differ, increment `count`.
4. Right shift both `start` and `goal` by one bit to process the next bits.
5. Return `count`.

### Time Complexity

- **O(n)**: Proportional to the number of bits in the larger of the two numbers.

### Space Complexity

- **O(1)**: Constant space usage.

### Code

```cpp
int solve(int strt, int goal) {
    int count = 0;
    while (strt != 0 || goal != 0) {
        int bit1 = strt & 1;
        int bit2 = goal & 1;
        if (bit1 != bit2) {
            count++;
        }
        strt >>= 1;
        goal >>= 1;
    }
    return count;
}

int minBitFlips(int start, int goal) {
    return solve(start, goal);
}
```

## Approach 2: XOR to Find Different Bits

### Algorithm

1. XOR `start` and `goal` to get a value that indicates differing bits.
2. Initialize `count` as `0`.
3. Use a loop to count the number of set bits (1s) in the XOR result:
   - Increment `count` for each set bit.
   - Right shift the XOR result to process the next bit.
4. Return `count`.

### Time Complexity

- **O(n)**: Proportional to the number of bits in the larger of the two numbers.

### Space Complexity

- **O(1)**: Constant space usage.

### Code

```cpp
int minBitFlips(int start, int goal) {
    int temp = start ^ goal;
    int count = 0;

    while (temp) {
        count += temp & 1;  // Increment count if the least significant bit is 1
        temp >>= 1;         // Shift right to process the next bit
    }
    return count;
}
```

# 150. Single Number

### Problem Link

[LeetCode - Single Number](https://leetcode.com/problems/single-number/description/)

## Approach 1: Frequency Map

### Algorithm

1. Use a frequency map (unordered map) to keep track of the frequency of each number.
2. Traverse the frequency map and return the key that has a frequency of 1.

### Time Complexity

- **O(n)**: We traverse the list to build the frequency map and then traverse the map to find the unique number.

### Space Complexity

- **O(n)**: Space for the frequency map in the worst case.

### Code

```cpp
int singleNumber(vector<int>& nums) {
    unordered_map<int, int> freqMap;
    for (int num : nums) {
        freqMap[num]++;
    }
    for (const auto& entry : freqMap) {
        if (entry.second == 1) {
            return entry.first;
        }
    }
    return -1; // This line should never be reached if the input is guaranteed to have a single number
}
```

## Approach 2: Bit Manipulation (XOR)

### Algorithm

1. Initialize a variable `ans` to `0`.
2. Iterate through each number in the array:
   - Perform the XOR operation between `ans` and the current number.
3. Return `ans` as it will hold the unique number.

### Time Complexity

- **O(n)**: We traverse the list once.

### Space Complexity

- **O(1)**: Constant space usage.

### Code

```cpp
int singleNumber(std::vector<int>& nums) {
    int ans = 0;
    for (int i = 0; i < nums.size(); i++) {
        ans = ans ^ nums[i];
    }
    return ans;
}
```
