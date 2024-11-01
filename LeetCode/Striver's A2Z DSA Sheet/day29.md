# 141. Bit Manipulation

### Problem Link

[GeeksforGeeks - Bit Manipulation](https://www.geeksforgeeks.org/problems/bit-manipulation-1666686020/1)

## Approach 1: Using Bitset

### Algorithm

1. Convert the number to a binary representation using a bitset.
2. Get the ith bit (1-based indexing) from the bitset.
3. Set the ith bit (1-based indexing) to 1.
4. Convert the modified bitset back to an integer.
5. Clear the ith bit (1-based indexing) by resetting it to 0.
6. Convert the modified bitset back to an integer.
7. Print the results.

### Time Complexity

- **O(1)**: The operations are constant time.

### Space Complexity

- **O(1)**: Constant space is used for storing the bitset.

### Code

```cpp
void bitManipulation(int num, int i) {
    // Convert the number to a binary representation using a bitset
    bitset<32> bits(num);

    // Get the ith bit (1-based indexing)
    int ithBit = bits[i - 1]; // Access the (i-1)th position for 0-based indexing

    // Set the ith bit (1-based indexing)
    bits.set(i - 1); // This sets the ith bit to 1
    int setBitNum = (int)bits.to_ulong(); // Convert back to unsigned long and then to int

    // Clear the ith bit (1-based indexing)
    bits.reset(i - 1); // This resets the ith bit to 0
    int clearBitNum = (int)bits.to_ulong(); // Convert back to unsigned long and then to int

    // Print the results as required
    cout << ithBit << " " << setBitNum << " " << clearBitNum;
}
```

## Approach 2: Using Bitwise Operations

### Algorithm

1. Create a mask for the ith bit (1-based, so shift left by `(i - 1)`).
2. Get the ith bit using the mask.
3. Set the ith bit using the bitwise OR operation with the mask.
4. Clear the ith bit using the bitwise AND operation with the negated mask.
5. Print the results.

### Time Complexity

- **O(1)**: The operations are constant time.

### Space Complexity

- **O(1)**: Constant space is used.

### Code

```cpp
void bitManipulation(int num, int i) {
    // Create a mask for the ith bit (1-based, so we shift left by (i-1))
    int mask = 1 << (i - 1);

    // Get the ith bit
    int ithBit = (num & mask) ? 1 : 0;

    // Set the ith bit
    int setBitNum = num | mask;

    // Clear the ith bit
    int clearBitNum = num & ~mask;

    // Print the results as required
    cout << ithBit << " " << setBitNum << " " << clearBitNum;
}
```

# 142. Check Whether K-th Bit is Set or Not

### Problem Link

[Naukri - Check Whether K-th Bit is Set or Not](https://www.naukri.com/code360/problems/check-whether-k-th-bit-is-set-or-not_5026446)

## Approach 1: Bit Counting

### Algorithm

1. Initialize a counter to zero.
2. While `n` is greater than zero:
   - Increment the counter.
   - If the counter equals `k`, return whether the least significant bit (LSB) is 1.
   - Divide `n` by 2 to shift right.
3. If the loop ends without finding the k-th bit, return false.

### Time Complexity

- **O(log n)**: Loop runs while `n` has bits.

### Space Complexity

- **O(1)**: Constant space used.

### Code

```cpp
bool isKthBitSet(int n, int k) {
    int count = 0;
    while (n > 0) {
        count++;
        if (count == k) {
            return (n % 2) == 1; // Check if k-th bit is 1
        }
        n /= 2;
    }
    return false; // k-th bit is not set if out of range
}
```

## Approach 2: Bitwise AND

### Algorithm

1. Use a bitwise AND operation to check the k-th bit.
2. Shift 1 to the left by `(k - 1)` to create a mask.
3. Return whether the result of the AND operation is non-zero.

### Time Complexity

- **O(1)**: This approach uses a direct bitwise operation.

### Space Complexity

- **O(1)**: Constant space is used.

### Code

```cpp
bool isKthBitSet(int n, int k) {
    if((n & (1 << (k - 1))) != 0){
        return true;
    }else{
        return false;
    }
}
```

## Approach 3: Right Shift and AND

### Algorithm

1. Right shift `n` by `(k - 1)` to bring the k-th bit to the least significant bit (LSB) position.
2. Use a bitwise AND operation with 1 to check if the k-th bit is set.
3. Return true if the result is 1, otherwise return false.

### Time Complexity

- **O(1)**: This approach uses a direct bitwise operation.

### Space Complexity

- **O(1)**: Constant space is used.

### Code

```cpp
bool isKthBitSet(int n, int k) {
    if(((n >> (k - 1)) & 1) == 1){
        return true;
    }else{
        return false;
    }
}
```

# 143. Odd or Even

### Problem Link

[GeeksforGeeks - Odd or Even](https://www.geeksforgeeks.org/problems/odd-or-even3618/1)

## Approach 1: Modulo Operator

### Algorithm

1. Check if the number `n` is divisible by `2` using the modulo operator.
2. If `n % 2 == 0`, return "Even".
3. Otherwise, return "Odd".

### Time Complexity

- **O(1)**: The operation takes constant time.

### Space Complexity

- **O(1)**: No extra space is used.

### Code

```cpp
string oddEven(int n) {
    return (n % 2 == 0) ? "Even" : "Odd";
}
```

## Approach 2: Using Bit Manipulation

### Algorithm

1. Use the property that if the last bit of a number is set, the number is odd; otherwise, it is even.
2. Check the last bit using `n & 1`.
3. If the last bit is `1`, return "Odd"; otherwise, return "Even".

### Time Complexity

- **O(1)**: The operation takes constant time.

### Space Complexity

- **O(1)**: No extra space is used.

### Code

```cpp
string oddEven(int n) {
    return (n & 1) ? "Odd" : "Even";
}
```

# Notes :

## Relationship Between \( N \) and \( N - 1 \) in Binary

When examining a number \( N \) and \( N - 1 \) in binary, there's an interesting bitwise relationship:

1. **Binary Representation of \( N \)**:

   - \( N \) will have its bits in a specific arrangement, with the rightmost 1-bit (the least significant 1) at a particular position.

2. **Binary Representation of \( N - 1 \)**:
   - Subtracting 1 from \( N \) changes the least significant 1 in \( N \) to 0 and flips all bits to the right of this bit to 1.

### Example

Consider \( N = 8 \) (which is \( 1000 \) in binary):

- \( N = 1000 \)
- \( N - 1 = 0111 \)

#### Breakdown:

- The least significant 1-bit in \( N \) is at the 4th position.
- Subtracting 1 changes this bit to 0 and flips all bits to the right, resulting in \( 0111 \).

### Practical Use

- **Bitwise AND Operation**: \( N \) & \( (N - 1) = 0 \)
  - Since \( N - 1 \) flips all bits at and to the right of the least significant 1-bit in \( N \), the bitwise AND between \( N \) and \( N - 1 \) is always zero.

This property is particularly useful for checking if \( N \) is a power of 2:

- If \( N \) & \( (N - 1) = 0 \), then \( N \) is a power of 2.

# 144. Power of Two

### Problem Link

[LeetCode - Power of Two](https://leetcode.com/problems/power-of-two/description/)

## Approach 1: Using Power Function

### Algorithm

1. Iterate from `0` to `30`.
2. For each `i`, check if \( n \) is equal to \( 2^i \) using the `pow` function.
3. If a match is found, return `true`.
4. If no matches are found after the loop, return `false`.

### Time Complexity

- **O(log n)**: The loop runs a fixed number of times (31 iterations).

### Space Complexity

- **O(1)**: Constant space is used.

### Code

```cpp
bool isPowerOfTwo(int n) {
    for (int i = 0; i <= 30; i++) {
        if (n == pow(2, i)) {
            return true;
        }
    }
    return false;
}
```

## Approach 2: Count Set Bits

### Explanation

1. A number \( N \) that is a power of two has exactly one `1` bit in its binary representation.
2. Count the number of set bits (1s) in the binary representation of \( N \).
3. If the count of set bits is exactly one, then \( N \) can be represented as a power of two.

### Algorithm

1. Initialize a counter to zero.
2. Use a loop to count the number of set bits in \( n \):
   - Increment the counter for each `1` bit encountered.
   - If the count exceeds one, return `false`.
3. After the loop, return `true` if the count is one, otherwise return `false`.

### Time Complexity

- **O(log n)**: The number of iterations is proportional to the number of bits in \( n \).

### Space Complexity

- **O(1)**: Constant space is used.

### Code

```cpp
bool isPowerOfTwo(int n) {
    int count = 0;
    while (n > 0) {
        count += (n & 1); // Increment count if the last bit is 1
        if (count > 1) return false; // More than one set bit
        n >>= 1; // Right shift to check the next bit
    }
    return count == 1; // Return true if there's exactly one set bit
}
```

## Approach 3: Bit Manipulation

### Explanation

1. Utilize the property of binary representation:
   - A number \( N \) that is a power of two has exactly one `1` bit in its binary representation.
   - Subtracting `1` from \( N \) will flip the least significant `1` bit to `0` and all bits to the right of it to `1`.
   - Thus, \( N \& (N - 1) \) will be `0` if \( N \) is a power of two.

### Algorithm

1. Check if \( n > 0 \) and if \( n \& (n - 1) \) is equal to `0`.
2. Return `true` if both conditions are satisfied; otherwise, return `false`.

### Time Complexity

- **O(1)**: The operation involves a constant time bitwise operation.

### Space Complexity

- **O(1)**: Constant space is used.

### Code

```cpp
bool isPowerOfTwo(int n) {
    return n > 0 && (n & (n - 1)) == 0; // Check if n is a power of two
}
```

# 145. Counting Bits

### Problem Link

[LeetCode - Counting Bits](https://leetcode.com/problems/counting-bits/description/)

## Approach 1: Counting Set Bits Using Right Shift

### Algorithm

1. Define a function `countSetBits` to count the number of set bits in a given integer `num`.
2. Initialize a counter to zero.
3. While `num` is not zero:
   - Increment the counter if the last bit is `1` using `num & 1`.
   - Right shift `num` by one to process the next bit.
4. Return the count.
5. In the `countBits` function, initialize a result vector of size `n + 1`.
6. Iterate from `0` to `n` and fill the result vector by calling `countSetBits` for each index.
7. Return the result vector.

### Time Complexity

- **O(n \* log n)**: The loop runs `n` times, and counting bits takes `log n` time.

### Space Complexity

- **O(n)**: Space is used for the result vector.

### Code

```cpp
class Solution {
public:
    int countSetBits(int num) {
        int count = 0;

        while (num != 0) {
            count += (num & 1); // Increment count if last bit is 1
            num >>= 1; // Right shift to process the next bit
        }

        return count;
    }

    vector<int> countBits(int n) {
        vector<int> result(n + 1);
        for (int i = 0; i <= n; i++) {
            result[i] = countSetBits(i);
        }

        return result;
    }
};
```

## Approach 2: Counting Set Bits Using Bit Manipulation

### Algorithm

1. Define a function `countSetBits` to count the number of set bits in a given integer `num`.
2. Initialize a counter to zero.
3. While `num` is not zero:
   - Increment the counter.
   - Use `num &= (num - 1)` to clear the least significant `1` bit.
4. Return the count.
5. In the `countBits` function, initialize a result vector of size `n + 1`.
6. Iterate from `0` to `n` and fill the result vector by calling `countSetBits` for each index.
7. Return the result vector.

### Time Complexity

- **O(n \* log n)**: The loop runs `n` times, and counting bits takes `log n` time.

### Space Complexity

- **O(n)**: Space is used for the result vector.

### Code

```cpp
int countSetBits(int num) {
    int count = 0;

    while (num) {
        count++;
        num &= (num - 1);
    }

    return count;
}

vector<int> countBits(int n) {
    vector<int> result(n + 1);
    for (int i = 0; i <= n; i++) {
        result[i] = countSetBits(i);
    }

    return result;
}
```

## Approach 3: Dynamic Programming

### Algorithm

1. Initialize a result vector of size `n + 1`.
2. Iterate from `1` to `n`:
   - Use the property that the number of set bits in `i` is equal to the number of set bits in `i / 2` plus the last bit (`i & 1`).
   - Set `result[i] = result[i >> 1] + (i & 1)`.
3. Return the result vector.

### Time Complexity

- **O(n)**: The loop runs `n` times.

### Space Complexity

- **O(n)**: Space is used for the result vector.

### Code

```cpp
vector<int> countBits(int n) {
    vector<int> result(n + 1); // Initialize the result vector of size n + 1

    for (int i = 1; i <= n; i++) {
        // Use the property that the number of set bits in i is
        // equal to the number of set bits in i/2 plus the last bit (i & 1)
        result[i] = result[i >> 1] + (i & 1);
    }

    return result; // Return the final result vector
}
```
