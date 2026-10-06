# Dynamic Programming

Dynamic Programming (DP) solves problems by storing solutions to smaller subproblems so that they do not have to be calculated repeatedly.

DP is especially useful when a problem has:

- **Overlapping subproblems**
- **Optimal substructure**

Two common approaches are:

1. Top-down — memoization
2. Bottom-up — tabulation

## 1. Fibonacci Without DP

The simple recursive implementation repeats calculations.

```cpp
int fibonacci(int n) {
    if (n <= 1) {
        return n;
    }

    return fibonacci(n - 1)
         + fibonacci(n - 2);
}
```

For larger `n`, many values are calculated repeatedly.

## 2. Fibonacci with Memoization

Memoization stores already calculated results.

```cpp
#include <iostream>
#include <vector>
using namespace std;

long long fibonacci(
    int n,
    vector<long long>& memo
) {
    if (n <= 1) {
        return n;
    }

    if (memo[n] != -1) {
        return memo[n];
    }

    memo[n] =
        fibonacci(n - 1, memo) +
        fibonacci(n - 2, memo);

    return memo[n];
}

int main() {
    int n = 10;

    vector<long long> memo(n + 1, -1);

    cout << fibonacci(n, memo);

    return 0;
}
```

### Output

```text
55
```

## 3. Fibonacci with Tabulation

Tabulation builds the solution from smaller values.

```cpp
#include <iostream>
#include <vector>
using namespace std;

long long fibonacci(int n) {
    if (n <= 1) {
        return n;
    }

    vector<long long> dp(n + 1);

    dp[0] = 0;
    dp[1] = 1;

    for (int i = 2; i <= n; i++) {
        dp[i] = dp[i - 1] + dp[i - 2];
    }

    return dp[n];
}

int main() {
    cout << fibonacci(10);

    return 0;
}
```

### Output

```text
55
```

## 4. Climbing Stairs

Suppose you can climb either one or two steps at a time.

For `n` steps:

```text
ways(n) = ways(n - 1) + ways(n - 2)
```

```cpp
#include <iostream>
#include <vector>
using namespace std;

int climbStairs(int n) {
    if (n <= 2) {
        return n;
    }

    vector<int> dp(n + 1);

    dp[1] = 1;
    dp[2] = 2;

    for (int i = 3; i <= n; i++) {
        dp[i] = dp[i - 1] + dp[i - 2];
    }

    return dp[n];
}

int main() {
    cout << climbStairs(5);

    return 0;
}
```

### Output

```text
8
```

## 5. 0/1 Knapsack

Each item can either be selected or not selected.

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

int knapsack(
    int capacity,
    const vector<int>& weights,
    const vector<int>& values
) {
    int n = weights.size();

    vector<vector<int>> dp(
        n + 1,
        vector<int>(capacity + 1, 0)
    );

    for (int i = 1; i <= n; i++) {
        for (int w = 0; w <= capacity; w++) {
            dp[i][w] = dp[i - 1][w];

            if (weights[i - 1] <= w) {
                dp[i][w] = max(
                    dp[i][w],
                    values[i - 1] +
                    dp[i - 1][w - weights[i - 1]]
                );
            }
        }
    }

    return dp[n][capacity];
}

int main() {
    vector<int> weights = {
        10, 20, 30
    };

    vector<int> values = {
        60, 100, 120
    };

    cout << knapsack(
        50,
        weights,
        values
    );

    return 0;
}
```

### Output

```text
220
```

## Memoization vs Tabulation

|Feature|Memoization|Tabulation|
|---|---|---|
|Approach|Top-down|Bottom-up|
|Usually uses|Recursion|Loops|
|Stores results|Yes|Yes|
|Computes|Needed states|Usually states in order|
|Recursion stack|Yes|No|

## Key Points

- Store results of subproblems.
- Avoid repeated computation.
- Identify the DP state carefully.
- Define the recurrence.
- Establish base cases.
- Choose memoization or tabulation.
