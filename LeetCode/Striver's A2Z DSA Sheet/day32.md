# 156. Count Primes

### Problem Link

[LeetCode - Count Primes](https://leetcode.com/problems/count-primes/description/)

## Approach 1: Brute Force (Checking Primality for Each Number)

### Algorithm

1. Define an `isPrime` function to check if a number is prime by dividing it with numbers up to itself.
2. Iterate through numbers from 2 to \( n-1 \), and for each number, check if it is prime using `isPrime`.
3. Count all numbers identified as prime.

### Time Complexity

- **O(n sqrt{n})**: For each number up to \( n-1 \), checking primality takes \( O(sqrt{n}) \).

### Space Complexity

- **O(1)**: No additional space is required.

### Code

```cpp
bool isPrime(int n) {
    if (n <= 1) {
        return false;
    }
    for (int i = 2; i < n; i++) {
        if (n % i == 0) {
            return false;
        }
    }
    return true;
}

int countPrimes(int n) {
    int count = 0;
    for (int i = 2; i < n; i++) {
        if (isPrime(i)) {
            count++;
        }
    }
    return count;
}
```

## Approach 2: Basic Sieve of Eratosthenes

### Algorithm

1. Create a boolean array `prime` of size \( n \), initialized to `true`.
2. Iterate `i` from 2 up to \( n-1 \):
   - If `prime[i]` is `true`, increment the count and mark all multiples of `i` as non-prime by setting `prime[j]` to `false` for each \( j = 2 \times i, 3 \times i, \dots \).
3. Return the count of numbers marked as prime.

### Time Complexity

- **O(n log n)**: The sieve operation for marking non-primes is \( O(n log n) \).

### Space Complexity

- **O(n)**: The boolean array `prime` of size \( n \) is used.

### Code

```cpp
int countPrimes(int n) {
    int count = 0;
    vector<int> prime(n, true);
    for (int i = 2; i < n; i++) {
        if (prime[i]) {
            count++;
            for (int j = 2 * i; j < n; j += i) {
                prime[j] = false;
            }
        }
    }
    return count;
}
```

## Approach 3: Optimized Sieve of Eratosthenes

### Algorithm

1. Initialize a boolean array `prime` of size \( n \) with all elements set to `true`.
2. Iterate `i` from 2 up to \(sqrt{n}\):
   - If `prime[i]` is `true`, mark all multiples of `i` starting from \( i times i \) as non-prime.
3. Count all remaining numbers marked as prime.

### Time Complexity

- **O(n log(log n))**: This is the optimized time complexity for the Sieve of Eratosthenes.

### Space Complexity

- **O(n)**: The boolean array `prime` of size \( n \) is used.

### Code

```cpp
int countPrimes(int n) {
    if (n <= 2) return 0;
    vector<bool> prime(n, true);
    int count = 0;
    for (int i = 2; i * i < n; i++) {
        if (prime[i]) {
            for (int j = i * i; j < n; j += i) {
                prime[j] = false;
            }
        }
    }
    for (int i = 2; i < n; i++) {
        if (prime[i]) count++;
    }
    return count;
}
```

# 157. Prime Factorization using Sieve

### Problem Link

[GeeksforGeeks - Prime Factorization using Sieve](https://www.geeksforgeeks.org/problems/prime-factorization-using-sieve/1)

## Approach 1: Trial Division

### Idea/Intuition

The idea is to start dividing \( N \) from the smallest prime number, 2, up to the square root of \( N \), as any factor larger than the square root would have already been divided out by a smaller factor. By repeatedly dividing \( N \) by each divisor, we efficiently collect all prime factors without needing to check numbers larger than \( sqrt(N) \) for divisibility.

### Algorithm

1. Initialize an empty vector `factors` to store prime factors.
2. Loop `i` from 2 up to the square root of \( N \):
   - While \( N \) is divisible by `i`, add `i` to `factors` and divide \( N \) by `i`.
3. If \( N \) is greater than 1 after the loop, add \( N \) as a factor.
4. Return `factors`.

### Time Complexity

- **O(sqrt(N))**: For each divisor up to \( sqrt(N) \), divide \( N \) fully by the divisor.

### Space Complexity

- **O(log N)**: Space for storing the prime factors.

### Code

```cpp
vector<int> findPrimeFactors(int N) {
    vector<int> factors;
    for (int i = 2; i * i <= N; i++) {
        while (N % i == 0) {
            factors.push_back(i);
            N /= i;
        }
    }
    if (N > 1) {
        factors.push_back(N);
    }
    return factors;
}
```

## Approach 2: Sieve of Eratosthenes for Smallest Prime Factor (SPF)

### Idea/Intuition

The Sieve of Eratosthenes can be modified to find the smallest prime factor (SPF) for each number up to \( N \). This allows for rapid prime factorization by simply dividing \( N \) by its smallest prime factor until \( N \) becomes 1. This approach avoids redundant calculations by marking each number's smallest prime factor in a preprocessing step, making factorization queries efficient.

### Algorithm

1. Initialize an array `spf` where `spf[i]` represents the smallest prime factor of `i`.
2. Set `spf[i] = i` for each `i` from 2 to \( N \).
3. For each number `i` from 2 up to \( \sqrt{N} \):
   - If `i` is prime (i.e., `spf[i] == i`), mark its multiples by setting `spf[j] = i`.
4. Using `spf`, repeatedly divide \( N \) by `spf[N]` and add `spf[N]` to `factors` until \( N \) becomes 1.
5. Return `factors`.

### Time Complexity

- **O(N log(log N))**: The Sieve of Eratosthenes time complexity for setting up `spf`.

### Space Complexity

- **O(N)**: Array `spf` of size \( N+1 \) is used.

### Code

```cpp
vector<int> findPrimeFactors(int N) {
    vector<int> spf(N + 1); // spf[i] will store smallest prime factor of i
    for (int i = 2; i <= N; i++) {
        spf[i] = i;
    }

    for (int i = 2; i * i <= N; i++) {
        if (spf[i] == i) { // i is prime
            for (int j = i * i; j <= N; j += i) {
                if (spf[j] == j) {
                    spf[j] = i;
                }
            }
        }
    }

    vector<int> factors;
    while (N > 1) {
        factors.push_back(spf[N]);
        N /= spf[N];
    }

    return factors;
}
```

# 158. Pow(x, n)

### Problem Link

[LeetCode - Pow(x, n)](https://leetcode.com/problems/powx-n/)

## Approach: Exponentiation by Squaring

### Idea/Intuition

Exponentiation by squaring is an efficient way to compute powers. The approach divides the exponent `n` by 2 in each step, squaring the base `x` when `n` is even. If `n` is odd, we multiply the result by `x` and reduce `n` by 1. This allows us to compute \( x^n \) in logarithmic time by minimizing the number of multiplications.

### Algorithm

1. Initialize `ans` as 1.0 to store the result.
2. If `n` is negative, take the reciprocal of `x` and make `n` positive.
3. While `n` is greater than 0:
   - If `n` is odd, multiply `ans` by `x` and decrease `n` by 1.
   - If `n` is even, square `x` and halve `n`.
4. Return `ans`.

### Time Complexity

- **O(log n)**: The exponent is reduced by half in each iteration, resulting in logarithmic time complexity.

### Space Complexity

- **O(1)**: Constant space is used.

### Code

```cpp
double myPow(double x, int n) {
    double ans = 1.0;
    long long power = n;

    if (power < 0) {
        x = 1 / x;
        power = -power;
    }

    while (power > 0) {
        if (power % 2 == 1) {
            ans *= x;
            power -= 1;
        } else {
            x *= x;
            power /= 2;
        }
    }

    return ans;
}
```

# 159. Stack Implementation using Array

### Problem Link

[Naukri - Stack Implementation using Array](https://www.naukri.com/code360/problems/stack-implementation-using-array_3210209)

## Approach: Array-based Stack with Top Pointer

### Idea/Intuition

A stack follows the Last In First Out (LIFO) principle, where the most recently added element is removed first. We use an array and a variable `topIndex` to keep track of the index of the latest element in the stack. When elements are added, `topIndex` increments, and when elements are removed, `topIndex` decrements. This allows for efficient stack operations.

### Algorithm

1. **Initialize**:

   - Create an array of a specified `capacity`.
   - Initialize `topIndex` to -1, indicating an empty stack.

2. **push(x)**:
   - Increment `topIndex` and place the element at `arr[topIndex]` if the stack is not full.
3. **pop()**:

   - If the stack is not empty, return the element at `arr[topIndex]` and decrement `topIndex`.

4. **top()**:

   - Return the element at `arr[topIndex]` if the stack is not empty.

5. **isEmpty()**:

   - Return true if `topIndex` is -1, indicating the stack is empty.

6. **isFull()**:
   - Return true if `topIndex` equals `capacity - 1`, indicating the stack is full.

### Time Complexity

- **O(1)** for `push`, `pop`, `top`, `isEmpty`, and `isFull` operations.

### Space Complexity

- **O(N)**: For an array of size `N` to hold stack elements.

### Code

```cpp
// Stack class.
class Stack {
    int* arr;
    int topIndex;
    int capacity;

public:

    Stack(int capacity) {
        this->capacity = capacity;
        arr = new int[capacity];
        topIndex = -1;
    }

    void push(int num) {
        if (topIndex < capacity - 1) {  // Check if stack is not full
            topIndex++;
            arr[topIndex] = num;
        }
    }

    int pop() {
        if (topIndex >= 0) {  // Check if stack is not empty
            int result = arr[topIndex];
            topIndex--;
            return result;
        }
        return -1;  // Return -1 if stack is empty
    }

    int top() {
        if (topIndex >= 0) {  // Check if stack is not empty
            return arr[topIndex];
        }
        return -1;  // Return -1 if stack is empty
    }

    int isEmpty() {
        return topIndex == -1;
    }

    int isFull() {
        return topIndex == capacity - 1;
    }
};
```

# 160. Implement Queue using Arrays

### Problem Link

[Naukri - Implement Queue using Arrays](https://www.naukri.com/code360/problems/implement-queue-using-arrays_8390825)

## Approach: Array-based Queue with Front and Rear Pointers

### Idea/Intuition

A queue follows the First In First Out (FIFO) principle, where the first element added is the first one to be removed. Using an array and two pointers (`front` and `rear`), we can efficiently keep track of the elements at both ends of the queue. Elements are added at the `rear` and removed from the `front`.

### Algorithm

1. **Initialize**:

   - Set `front` and `rear` pointers to `0`.
   - Declare an array `arr` with a fixed size (e.g., 100001) to store the elements.

2. **enqueue(e)**:

   - Insert the element `e` at `arr[rear]` and increment `rear` to point to the next position for the next enqueue operation.

3. **dequeue()**:
   - If the queue is empty (`front == rear`), return `-1`.
   - Otherwise, return the element at `arr[front]` and increment `front` to remove it from the queue.

### Time Complexity

- **O(1)** for `enqueue` and `dequeue` operations due to the direct manipulation of array indices.

### Space Complexity

- **O(N)**: An array of size `N` (fixed as 100001 here) is used to store the elements of the queue.

### Code

```cpp
class Queue {

    int front, rear;
    vector<int> arr;

public:
    Queue() {
        front = 0;
        rear = 0;
        arr.resize(100001);
    }

    // Enqueue (add) element 'e' at the end of the queue.
    void enqueue(int e) {
        arr[rear] = e;
        rear++;
    }

    // Dequeue (retrieve) the element from the front of the queue.
    int dequeue() {
        if (front == rear) {
            return -1;  // Queue is empty
        }
        int result = arr[front];
        front++;
        return result;
    }
};
```
