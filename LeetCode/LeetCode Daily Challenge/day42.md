# Prime Subtraction Operation

**Problem Link**: [Prime Subtraction Operation](https://leetcode.com/problems/prime-subtraction-operation/)

## Approach 1: Sieve of Eratosthenes with Sequential Adjustment

### Idea

1. Precompute all prime numbers up to the maximum value in the array using the **Sieve of Eratosthenes**.
2. Iterate through the array and try to subtract prime numbers from each element to make it strictly less than the previous element.
3. If no valid subtraction is possible, return `false`.

### Algorithm

1. **Precompute Primes**:
   - Use the Sieve of Eratosthenes to identify all prime numbers up to the maximum value in `nums`.
2. **Iterate and Adjust**:
   - Maintain a `currentValue` starting from `1`.
   - For each element in `nums`, subtract the largest possible prime such that the result is greater than `currentValue`.
   - If no valid subtraction is found, return `false`.
3. Return `true` if all elements can be adjusted.

### Time Complexity

- **Sieve Preprocessing**: **O(m \* log(log(m)))**, where `m` is the maximum element in `nums`.
- **Adjustment Loop**: **O(n \* m)**, where `n` is the size of the array and `m` is the average number of primes below each number.

### Space Complexity

- **O(m)**, for storing the prime numbers.

### Code Implementation

```cpp
class Solution {
public:
    bool primeSubOperation(vector<int>& nums) {
        int maxElement = *max_element(nums.begin(), nums.end());

        // Create Sieve of Eratosthenes array to identify prime numbers
        vector<bool> sieve(maxElement + 1, true);
        sieve[1] = false;
        for (int i = 2; i <= sqrt(maxElement); i++) {
            if (sieve[i]) {
                for (int j = i * i; j <= maxElement; j += i) {
                    sieve[j] = false;
                }
            }
        }

        int currValue = 1; // Start with the minimum possible value
        for (int num : nums) {
            int difference = num - currValue;

            // Check if we can subtract a prime to make num greater than currValue
            while (difference > 0 && !sieve[difference]) {
                --difference;
            }

            if (difference == 0) {
                return false; // No valid prime subtraction found
            }

            currValue = num - difference; // Update currValue for the next iteration
        }

        return true;
    }
};
```

## Approach 2: Sieve of Eratosthenes with Iterative Adjustment

### Idea

1. Precompute all prime numbers less than 1000 using the **Sieve of Eratosthenes**.
2. Iterate through the array from the second last element to the beginning.
3. For each element, check if subtracting a prime number can make it strictly less than the next element. If not, return `false`.

### Algorithm

1. **Sieve Preprocessing**:
   - Precompute all prime numbers less than 1000 into an `isPrime` array.
2. **Iterate Backward**:
   - If the current element is already less than the next, skip.
   - Otherwise, find the largest prime that, when subtracted from the current element, makes it less than the next.
   - If no such prime exists, return `false`.
3. Return `true` if the array can be made strictly increasing.

### Time Complexity

- **Sieve Preprocessing**: **O(1)**, precomputes primes up to a fixed limit.
- **Adjustment Loop**: **O(n \* m)**, where `n` is the size of the array and `m` is the maximum value in `nums`.

### Space Complexity

- **O(1)**, uses constant additional space for the `isPrime` array.

### Code Implementation

```cpp
class Solution {
public:
    bool isPrime[1000];

    void sieve() {
        fill(isPrime, isPrime + 1000, true);
        isPrime[0] = false; // 0 is not a prime number
        isPrime[1] = false; // 1 is not a prime number

        for (int i = 2; i * i < 1000; i++) {
            if (isPrime[i]) {
                for (int j = i * i; j < 1000; j += i) {
                    isPrime[j] = false;
                }
            }
        }
    }

    bool primeSubOperation(vector<int>& nums) {
        int n = nums.size();
        sieve(); // Precompute primes

        for (int i = n - 2; i >= 0; i--) {
            if (nums[i] < nums[i + 1]) {
                continue;
            }

            // Attempt to decrease nums[i] using primes
            for (int p = 2; p < nums[i]; p++) {
                if (!isPrime[p]) {
                    continue;
                }

                if (nums[i] - p < nums[i + 1]) {
                    nums[i] -= p;
                    break;
                }
            }

            // If nums[i] could not be reduced below nums[i+1], return false
            if (nums[i] >= nums[i + 1]) {
                return false;
            }
        }

        return true;
    }
};
```
