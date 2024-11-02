# 151. Subsets

### Problem Link

[LeetCode - Subsets](https://leetcode.com/problems/subsets/description/)

## Approach: Bit Manipulation

### Algorithm

1. Initialize an empty vector `allSubsets` to store all possible subsets.
2. Calculate the total number of subsets, which is \(2^{\text{nums.size()}}\).
3. Loop through all possible subset IDs (0 to `numOfSubsets - 1`).
   - For each subset ID, initialize an empty subset and a bitmask set to the subset ID.
   - For each bit in the bitmask, if it is set, add the corresponding element from `nums` to the subset.
4. Add each subset to `allSubsets`.
5. Return `allSubsets`.

### Time Complexity

- **O(n \* 2^n)**: We generate \(2^n\) subsets, and for each subset, we potentially include each of the \(n\) elements.

### Space Complexity

- **O(n \* 2^n)**: Space required to store all subsets.

### Code

```cpp
vector<vector<int>> subsets(vector<int>& nums) {
    vector<vector<int>> allSubsets;
    int numOfSubsets = pow(2, nums.size());

    for (int subsetID = 0; subsetID < numOfSubsets; subsetID++) {
        vector<int> subset;
        int bitmask = subsetID;

        for (int i = 0; bitmask; i++, bitmask >>= 1) {
            if (bitmask & 1) {
                subset.push_back(nums[i]);
            }
        }

        allSubsets.push_back(subset);
    }

    return allSubsets;
}
```

# XOR Pattern for Numbers from 1 to N

This pattern allows you to calculate the XOR of all numbers from 1 to \( N \) without looping, which is efficient for large values of \( N \). The XOR operation has a periodic pattern that repeats every 4 numbers, which simplifies the calculation.

## Explanation of XOR Pattern

The pattern for XOR from 1 to \( N \) depends on the remainder when \( N \) is divided by 4. Let's break it down into four cases:

### Cases for XOR Pattern:

1. **When N mod 4 = 0:**

   - **Formula:** XOR(1 to N) = N
   - **Explanation:** If N is divisible by 4, the XOR of all numbers from 1 to N is simply N itself.
   - **Example:** For N = 4, XOR(1, 2, 3, 4) = 4.

2. **When N mod 4 = 1:**

   - **Formula:** XOR(1 to N) = 1
   - **Explanation:** If the remainder when N is divided by 4 is 1, the XOR result of all numbers from 1 to N will be 1.
   - **Example:** For N = 5, XOR(1, 2, 3, 4, 5) = 1.

3. **When N mod 4 = 2:**

   - **Formula:** XOR(1 to N) = N + 1
   - **Explanation:** If the remainder when N is divided by 4 is 2, the XOR result of all numbers from 1 to N will be N + 1.
   - **Example:** For N = 6, XOR(1, 2, 3, 4, 5, 6) = 7 (which is N + 1 = 6 + 1).

4. **When N mod 4 = 3:**

   - **Formula:** XOR(1 to N) = 0
   - **Explanation:** If the remainder when N is divided by 4 is 3, the XOR result of all numbers from 1 to N will be 0.
   - **Example:** For N = 7, XOR(1, 2, 3, 4, 5, 6, 7) = 0.

### Why This Pattern Occurs

The periodic nature of XOR in binary is what leads to this 4-cycle pattern. The XOR operation flips bits between 0 and 1. By observing the results for small values of \( N \) (e.g., 1, 2, 3, and 4), we can see that the result restarts in the same sequence every 4 numbers. This makes it possible to use modular arithmetic to quickly determine the XOR result.

### Practical Application

Using this pattern, you can calculate the XOR of numbers from 1 to \( N \) in constant time \( O(1) \) by simply checking \( N \mod 4 \) and applying the appropriate case. This is much faster than looping through each number and performing iterative XOR, especially for large \( N \) values.

This pattern is particularly useful in competitive programming and problems that involve bit manipulation, where calculating cumulative XOR in an optimized way is crucial.

# 152. L to R XOR

### Problem Link

[Naukri - L to R XOR](https://www.naukri.com/code360/problems/l-to-r-xor_8160412?)

## Approach 1: Brute Force XOR from L to R

### Algorithm

1. Initialize `ans` to `0`.
2. Traverse from `L` to `R`, and for each number `i`, update `ans` by performing `ans = ans ^ i`.
3. Return `ans` as the result after the loop.

### Time Complexity

- **O(R - L + 1)**: Linear in the range `[L, R]`.

### Space Complexity

- **O(1)**: Constant space usage.

### Code

```cpp
int findXOR(int L, int R) {
    int ans = 0;
    for (int i = L; i <= R; i++) {
        ans ^= i;
    }
    return ans;
}
```

## Approach 2: Pattern-Based XOR Using Properties

### Algorithm

1. Define a helper function `xorToN(N)` that calculates the XOR from `1` to `N` using specific rules based on the value of `N % 4`:
   - If `N % 4 == 0`, return `N`.
   - If `N % 4 == 1`, return `1`.
   - If `N % 4 == 2`, return `N + 1`.
   - If `N % 4 == 3`, return `0`.
2. To find the XOR from `L` to `R`, calculate `xorToN(R) ^ xorToN(L - 1)`, which gives the desired result without looping through all numbers.

### Time Complexity

- **O(1)**: The solution calculates XOR in constant time due to direct arithmetic operations based on patterns.

### Space Complexity

- **O(1)**: Constant space usage.

### Code

```cpp
int xorToN(int N) {
    if (N % 4 == 0) return N;
    if (N % 4 == 1) return 1;
    if (N % 4 == 2) return N + 1;
    return 0;
}

int findXOR(int L, int R) {
    return xorToN(R) ^ xorToN(L - 1);
}
```

# 153. Two Numbers with Odd Occurrences

### Problem Link

[Naukri Code360 - Two Numbers with Odd Occurrences](https://www.naukri.com/code360/problems/two-numbers-with-odd-occurrences_8160466)

## Approach 1: Frequency Map

1. Initialize a frequency map to store the occurrences of each number in the array.
2. Traverse through the array, updating the frequency count for each element.
3. Traverse through the frequency map to identify the two numbers that have an odd frequency.
4. Use a simple conditional check to return the two numbers in decreasing order.

### Time Complexity

- **O(n):** Traversing the array and building the frequency map.

### Space Complexity

- **O(n):** Space used by the frequency map.

### Code

```cpp
#include <vector>
#include <unordered_map>
using namespace std;

vector<int> twoOddNum(vector<int> arr) {
    unordered_map<int, int> freq;

    // Store the frequency of each number
    for (int num : arr) {
        freq[num]++;
    }

    vector<int> result;
    // Collect numbers with odd frequency
    for (auto &entry : freq) {
        if (entry.second % 2 != 0) {
            result.push_back(entry.first);
        }
    }

    // Return the result in decreasing order without sorting
    return result[0] > result[1] ? vector<int>{result[0], result[1]} : vector<int>{result[1], result[0]};
}
```

## Approach 2: Bit Manipulation with XOR

### Intuition

The XOR approach leverages the properties of XOR to identify two numbers with odd occurrences efficiently:

1. XORing identical numbers results in 0, so XORing all elements in the array cancels out pairs of identical numbers, leaving us with the XOR of the two numbers that appear an odd number of times.
2. To isolate the two numbers from this XOR result, we can find a bit position where these two numbers differ. This bit position is identified as the rightmost set bit of the XOR result. It indicates that one of the odd-occurring numbers has this bit set to 1, while the other has it set to 0.
3. Using this bit as a differentiator, we divide the numbers in the array into two groups. XORing the elements within each group gives the two odd-occurring numbers, as other elements cancel out.

### Algorithm

1. Initialize `xorAll` to 0 and XOR all elements in `arr`. This will yield the XOR of the two numbers that occur an odd number of times.
2. Identify the rightmost set bit in `xorAll` by using `xorAll & ~(xorAll - 1)`. This bit helps to separate the two numbers based on whether they have this bit set or not.
3. Initialize two variables, `num1` and `num2`, to 0.
4. Traverse through `arr`, dividing the elements into two groups based on the rightmost set bit:
   - XOR elements in each group to isolate each of the two numbers.
5. Return the two numbers in decreasing order.

### Time Complexity

- **O(n):** Single pass through the array to calculate XOR values and separate elements.

### Space Complexity

- **O(1):** Constant space usage.

### Code

```cpp
#include <vector>
using namespace std;

vector<int> twoOddNum(vector<int> arr) {
    int xorAll = 0;

    // XOR all elements to get XOR of the two odd-occurring numbers
    for (int num : arr) {
        xorAll ^= num;
    }

    // Find the rightmost set bit in xorAll
    int rightmostSetBit = xorAll & ~(xorAll - 1);

    int num1 = 0, num2 = 0;
    // Separate elements into two groups based on the rightmost set bit
    for (int num : arr) {
        if (num & rightmostSetBit) {
            num1 ^= num;
        } else {
            num2 ^= num;
        }
    }

    // Return the result in decreasing order
    return {max(num1, num2), min(num1, num2)};
}
```

# 154. Prime Factors

### Problem Link

[GeeksforGeeks - Prime Factors](https://www.geeksforgeeks.org/problems/prime-factors5052/1)

## Approach 1: Brute Force Traversal

### Algorithm

1. Initialize an empty vector `ans` to store distinct prime factors of \( N \).
2. Traverse from \( i = 2 \) to \( N \).
   - If \( N \) is divisible by \( i \), check if \( i \) is prime.
   - If prime, add \( i \) to `ans`.
3. Return `ans`.

### Time Complexity

- **O(N \* sqrt{N})**: The outer loop runs \( N \) times, and primality check runs in \( O(sqrt{N}) \).

### Space Complexity

- **O(k)**: Space for storing \( k \) distinct prime factors.

### Code

```cpp
// Helper function to check if a number is prime
bool isPrime(int num) {
    if (num <= 1) return false;
    if (num <= 3) return true;
    if (num % 2 == 0 || num % 3 == 0) return false;
    for (int i = 5; i * i <= num; i += 6) {
        if (num % i == 0 || num % (i + 2) == 0)
            return false;
    }
    return true;
}

vector<int> AllPrimeFactors(int N) {
    vector<int> ans;
    for (int i = 2; i <= N; ++i) {
        if (N % i == 0 && isPrime(i)) {
            ans.push_back(i);
        }
    }
    return ans;
}
```

## Approach 2: Traverse from 2 to √N

### Algorithm

1. Initialize an empty vector `ans` to store distinct prime factors of \( N \).
2. Traverse from \( i = 2 \) to \( \sqrt{N} \).
   - If \( N \) is divisible by \( i \):
     - Check if \( i \) is prime.
     - If prime, add \( i \) to `ans`.
     - Calculate \( {other_factor} = frac{N}{i} \).
     - If \( {other_factor} \neq i \) and is prime, add it to `ans`.
3. Return `ans`.

### Time Complexity

- **O(sqrt{N} times sqrt{N}) = O(N)**: The outer loop runs \( O(sqrt{N}) \) times, and each primality check runs in \( O(sqrt{N}) \).

### Space Complexity

- **O(k)**: Space for storing \( k \) distinct prime factors.

### Code

```cpp
// Helper function to check if a number is prime
bool isPrime(int num) {
    if (num <= 1) return false;
    if (num <= 3) return true;
    if (num % 2 == 0 || num % 3 == 0) return false;
    for (int i = 5; i * i <= num; i += 6) {
        if (num % i == 0 || num % (i + 2) == 0)
            return false;
    }
    return true;
}

vector<int> AllPrimeFactors(int N) {
    vector<int> ans;
    for (int i = 2; i * i <= N; ++i) {
        if (N % i == 0) {
            if (isPrime(i)) {
                ans.push_back(i);
            }
            int other_factor = N / i;
            if (other_factor != i && isPrime(other_factor)) {
                ans.push_back(other_factor);
            }
        }
    }
    return ans;
}
```

## Approach 3: Factorization with Division

### Algorithm

1. Initialize an empty vector `ans` to store distinct prime factors of \( N \).
2. Start with \( i = 2 \).
3. While \( i times i <= N \):
   - If \( N \) is divisible by \( i \):
     - Check if \( i \) is prime. If yes, add \( i \) to `ans`.
     - Divide \( N \) by \( i \) repeatedly until \( N \) is no longer divisible by \( i \).
4. If \( N > 1 \) after the loop, \( N \) itself is prime. Add \( N \) to `ans`.
5. Return `ans`.

### Time Complexity

- **O(sqrt{N})**: The outer loop runs \( O(sqrt{N}) \) times, and each division step reduces \( N \).

### Space Complexity

- **O(k)**: Space for storing \( k \) distinct prime factors.

### Code

```cpp
// Helper function to check if a number is prime
bool isPrime(int num) {
    if (num <= 1) return false;
    if (num <= 3) return true;
    if (num % 2 == 0 || num % 3 == 0) return false;
    for (int i = 5; i * i <= num; i += 6) {
        if (num % i == 0 || num % (i + 2) == 0)
            return false;
    }
    return true;
}

vector<int> AllPrimeFactors(int N) {
    vector<int> ans;
    for (int i = 2; i * i <= N; ++i) {
        while (N % i == 0) {
            if (isPrime(i)) {
                ans.push_back(i);
            }
            N /= i;
        }
    }
    if (N > 1 && isPrime(N)) {
        ans.push_back(N);
    }
    return ans;
}
```

## Approach 4: Factorization with Edge Case

### Algorithm

1. Initialize an empty vector `ans` to store distinct prime factors of \( N \).
2. Start with \( i = 2 \).
3. While \( i \times i <= N \):
   - If \( N \) is divisible by \( i \):
     - Add \( i \) to `ans` if it is prime.
     - Divide \( N \) by \( i \) repeatedly until \( N \) is no longer divisible by \( i \).
4. After the loop, if \( N > 1 \), add \( N \) to `ans` as it is prime.
5. Return `ans`.

### Time Complexity

- **O(sqrt{N})**: The outer loop runs \( O(sqrt{N}) \) times, and each division step reduces \( N \).

### Space Complexity

- **O(k)**: Space for storing \( k \) distinct prime factors.

### Code

```cpp
// Helper function to check if a number is prime
bool isPrime(int num) {
    if (num <= 1) return false;
    if (num <= 3) return true;
    if (num % 2 == 0 || num % 3 == 0) return false;
    for (int i = 5; i * i <= num; i += 6) {
        if (num % i == 0 || num % (i + 2) == 0)
            return false;
    }
    return true;
}

vector<int> AllPrimeFactors(int N) {
    vector<int> ans;
    for (int i = 2; i * i <= N; ++i) {
        while (N % i == 0) {
            if (isPrime(i)) {
                ans.push_back(i);
            }
            N /= i;
        }
    }
    if (N > 1) {
        ans.push_back(N);
    }
    return ans;
}
```

# 155. Print All Divisors of a Number

### Problem Link

[Naukri - All Divisors of a Number](https://www.naukri.com/code360/problems/print-all-divisors-of-a-number_1164188)

## Approach 1: Simple Iteration

### Algorithm

1. Initialize a dynamic array to store the divisors of \( N \).
2. Loop from \( i = 1 \) to \( N \):
   - If \( N \) is divisible by \( i \) (i.e., \( N \mod i = 0 \)), store \( i \) in the array.
3. Return the array containing all divisors.

### Time Complexity

- **O(N)**: We iterate from 1 to \( N \) to find all divisors.

### Space Complexity

- **O(N)**: In the worst case, we may store all numbers from 1 to \( N \) as divisors.

### Code

```cpp
int* printDivisors(int n, int &size) {
    size = 0;
    int* divisors = new int[n]; // Allocate memory for divisors (worst case, all numbers are divisors)

    // Approach 1: Simple Iteration from 1 to N
    for (int i = 1; i <= n; ++i) {
        if (n % i == 0) {
            divisors[size++] = i; // Store the divisor
        }
    }

    return divisors; // Return the array of divisors
}
```

## Approach 2: Efficient Iteration Up to √N

### Algorithm

1. Initialize a dynamic array to store the divisors of \( N \).
2. Loop from \( i = 1 \) to \( sqrt{N} \):
   - If \( N \) is divisible by \( i \) (i.e., \( N \mod i = 0 \)):
     - Store \( i \) as a divisor.
     - Check if \( n/i \) is not equal to \( i \) to avoid duplication and store it as well.
3. Sort the array of divisors.
4. Return the sorted array containing all divisors.

### Time Complexity

- **O(√N log(√N))**: The divisor collection is O(√N), and sorting takes O(k log k), where k is the number of divisors (which is at most 2√N).

### Space Complexity

- **O(√N)**: We store at most two divisors for each integer up to √N.

### Code

```cpp
#include <cmath>
#include <algorithm>

int* printDivisors(int n, int &size) {
    size = 0;
    // Allocate enough space for divisors
    int* divisors = new int[2 * static_cast<int>(sqrt(n))];

    // Approach 2: Efficient Iteration Up to √N
    for (int i = 1; i <= static_cast<int>(sqrt(n)); ++i) {
        if (n % i == 0) {
            divisors[size++] = i; // Store the divisor
            if (i != n / i) { // Check to avoid duplication
                divisors[size++] = n / i; // Store the corresponding divisor
            }
        }
    }

    // Sort the array of divisors
    sort(divisors, divisors + size);

    return divisors; // Return the sorted array of divisors
}
```
