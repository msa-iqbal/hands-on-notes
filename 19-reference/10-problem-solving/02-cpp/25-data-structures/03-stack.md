# Stack

A **stack** is a linear data structure that follows **LIFO**:

> Last In, First Out

Example:

```text
Push 10
Push 20
Push 30

Top
 ↓
30
20
10
```

The last inserted element is removed first.

## 1. Using `std::stack`

```cpp
#include <iostream>
#include <stack>
using namespace std;

int main() {
    stack<int> numbers;

    numbers.push(10);
    numbers.push(20);
    numbers.push(30);

    cout << numbers.top() << endl;

    return 0;
}
```

### Output

```text
30
```

## 2. Push and Pop

```cpp
#include <iostream>
#include <stack>
using namespace std;

int main() {
    stack<int> numbers;

    numbers.push(10);
    numbers.push(20);
    numbers.push(30);

    while (!numbers.empty()) {
        cout << numbers.top() << " ";
        numbers.pop();
    }

    return 0;
}
```

### Output

```text
30 20 10
```

## 3. Stack Operations

```cpp
stack<int> s;

s.push(10);
s.push(20);

cout << s.top() << endl;
cout << s.size() << endl;

s.pop();

cout << s.top() << endl;

cout << boolalpha << s.empty() << endl;
```

### Output

```text
20
2
10
false
```

## 4. Reverse a String

A stack can be used to reverse a string.

```cpp
#include <iostream>
#include <stack>
using namespace std;

int main() {
    string text = "Hello";
    stack<char> s;

    for (char ch : text) {
        s.push(ch);
    }

    while (!s.empty()) {
        cout << s.top();
        s.pop();
    }

    cout << endl;

    return 0;
}
```

### Output

```text
olleH
```

## 5. Stack Using Vector

```cpp
#include <iostream>
#include <vector>
using namespace std;

class Stack {
private:
    vector<int> data;

public:
    void push(int value) {
        data.push_back(value);
    }

    void pop() {
        if (!data.empty()) {
            data.pop_back();
        }
    }

    int top() const {
        return data.back();
    }

    bool empty() const {
        return data.empty();
    }

    int size() const {
        return data.size();
    }
};

int main() {
    Stack s;

    s.push(10);
    s.push(20);
    s.push(30);

    cout << s.top() << endl;

    s.pop();

    cout << s.top() << endl;

    return 0;
}
```

### Output

```text
30
20
```

## Stack Operations

|Operation|Complexity|
|---|--:|
|`push()`|O(1)|
|`pop()`|O(1)|
|`top()`|O(1)|
|`empty()`|O(1)|
|`size()`|O(1)|

## Common Uses

- Function call stack
- Undo operations
- Expression evaluation
- Parentheses matching
- Backtracking
- Depth-first search
