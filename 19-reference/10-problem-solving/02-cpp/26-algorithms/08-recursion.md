# Recursion

**Recursion** is a technique where a function calls itself to solve a smaller version of the same problem.

A recursive function normally needs:

1. **Base case** — stops the recursion.

2. **Recursive case** — calls the function again with a smaller/simpler input.

## Basic Example

```cpp
#include <iostream>
using namespace std;

void countDown(int n) {
    if (n == 0) {
        return;
    }

    cout << n << " ";

    countDown(n - 1);
}

int main() {
    countDown(5);

    return 0;
}
```

### Output

```text
5 4 3 2 1
```

## How It Works

```text
countDown(5)
    ↓
countDown(4)
    ↓
countDown(3)
    ↓
countDown(2)
    ↓
countDown(1)
    ↓
countDown(0)
    ↓
return
```

## Factorial

Factorial:

```text
5! = 5 × 4 × 3 × 2 × 1
```

Recursive definition:

```text
n! = n × (n - 1)!
```

```cpp
#include <iostream>
using namespace std;

long long factorial(int n) {
    if (n <= 1) {
        return 1;
    }

    return n * factorial(n - 1);
}

int main() {
    cout << factorial(5) << endl;

    return 0;
}
```

### Output

```text
120
```

## Sum of Numbers

```cpp
#include <iostream>
using namespace std;

int sum(int n) {
    if (n == 0) {
        return 0;
    }

    return n + sum(n - 1);
}

int main() {
    cout << sum(5) << endl;

    return 0;
}
```

### Output

```text
15
```

## Fibonacci

```text
0 1 1 2 3 5 8 13 ...
```

```cpp
#include <iostream>
using namespace std;

int fibonacci(int n) {
    if (n <= 1) {
        return n;
    }

    return fibonacci(n - 1) + fibonacci(n - 2);
}

int main() {
    for (int i = 0; i < 10; i++) {
        cout << fibonacci(i) << " ";
    }

    return 0;
}
```

### Output

```text
0 1 1 2 3 5 8 13 21 34
```

## Reverse a String

```cpp
#include <iostream>
#include <string>
using namespace std;

void reverseString(
    const string& text,
    int index
) {
    if (index < 0) {
        return;
    }

    cout << text[index];

    reverseString(text, index - 1);
}

int main() {
    string text = "Hello";

    reverseString(text, text.size() - 1);

    return 0;
}
```

### Output

```text
olleH
```

## Recursive Array Sum

```cpp
#include <iostream>
using namespace std;

int arraySum(int numbers[], int size) {
    if (size == 0) {
        return 0;
    }

    return numbers[size - 1] +
           arraySum(numbers, size - 1);
}

int main() {
    int numbers[] = {10, 20, 30, 40};

    cout << arraySum(numbers, 4) << endl;

    return 0;
}
```

### Output

```text
100
```

## Recursive Binary Search

```cpp
#include <iostream>
using namespace std;

int binarySearch(
    int numbers[],
    int left,
    int right,
    int target
) {
    if (left > right) {
        return -1;
    }

    int middle = left + (right - left) / 2;

    if (numbers[middle] == target) {
        return middle;
    }

    if (numbers[middle] < target) {
        return binarySearch(
            numbers,
            middle + 1,
            right,
            target
        );
    }

    return binarySearch(
        numbers,
        left,
        middle - 1,
        target
    );
}

int main() {
    int numbers[] = {
        10, 20, 30, 40, 50
    };

    int result = binarySearch(
        numbers,
        0,
        4,
        40
    );

    cout << result << endl;

    return 0;
}
```

### Output

```text
3
```

## Recursion and the Call Stack

Each recursive call creates a new stack frame.

For:

```cpp
factorial(3);
```

the call sequence is:

```text
factorial(3)
    ↓
factorial(2)
    ↓
factorial(1)
```

Then the calls return:

```text
factorial(1) = 1
factorial(2) = 2
factorial(3) = 6
```

## Base Case Is Essential

Incorrect:

```cpp
int count(int n) {
    return count(n - 1);
}
```

There is no stopping condition, so recursion continues until the program exhausts the call stack.

Correct:

```cpp
int count(int n) {
    if (n == 0) {
        return 0;
    }

    return count(n - 1);
}
```

## Recursion vs Iteration

|Feature|Recursion|Iteration|
|---|---|---|
|Main mechanism|Function calls|Loops|
|Extra stack usage|Usually yes|Usually low|
|Code|Can be concise|Often straightforward|
|Tree algorithms|Very natural|Often more complex|
|Risk of stack overflow|Yes|Usually lower|

## Key Points

- Every recursive algorithm needs a terminating condition.
- Recursive calls should move toward the base case.
- Recursion uses the call stack.
- Trees and divide-and-conquer algorithms naturally use recursion.
- Excessive recursion can cause stack overflow.
