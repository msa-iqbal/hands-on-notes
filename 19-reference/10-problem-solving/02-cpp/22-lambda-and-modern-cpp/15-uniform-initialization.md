# Uniform Initialization

**Uniform initialization** uses brace syntax `{}` to initialize objects, arrays, containers, and class instances.

It was introduced with C++11.

## Example 1: Basic Variables

```cpp
#include <iostream>
using namespace std;

int main() {
    int number{10};
    double price{99.99};
    char letter{'A'};

    cout << number << endl;
    cout << price << endl;
    cout << letter << endl;

    return 0;
}
```

## Example 2: Array

```cpp
#include <iostream>
using namespace std;

int main() {
    int numbers[] {10, 20, 30, 40};

    for (int number : numbers) {
        cout << number << " ";
    }

    return 0;
}
```

### Output

```text
10 20 30 40
```

## Example 3: Vector

```cpp
#include <iostream>
#include <vector>
using namespace std;

int main() {
    vector<int> numbers{10, 20, 30, 40};

    for (int number : numbers) {
        cout << number << " ";
    }

    return 0;
}
```

## Example 4: Class Object

```cpp
#include <iostream>
using namespace std;

class Person {
public:
    string name;
    int age;
};

int main() {
    Person person{"Alice", 25};

    cout << person.name << endl;
    cout << person.age << endl;

    return 0;
}
```

## Example 5: Constructor

```cpp
#include <iostream>
#include <string>
using namespace std;

class Person {
private:
    string name;
    int age;

public:
    Person(string name, int age)
        : name(name), age(age) {}

    void show() const {
        cout << name << " " << age << endl;
    }
};

int main() {
    Person person{"Alice", 25};

    person.show();

    return 0;
}
```

## Example 6: Preventing Narrowing

Brace initialization prevents many narrowing conversions.

```cpp
int number{10.5}; // Error
```

Whereas traditional initialization may allow conversion:

```cpp
int number = 10.5;
```

The latter results in a converted integer value.

## Example 7: Nested Initialization

```cpp
#include <iostream>
using namespace std;

int main() {
    int matrix[2][2]{
        {1, 2},
        {3, 4}
    };

    for (const auto& row : matrix) {
        for (int value : row) {
            cout << value << " ";
        }

        cout << endl;
    }

    return 0;
}
```

### Output

```text
1 2
3 4
```

## initializer_list Consideration

Brace initialization can prefer constructors taking `std::initializer_list`.

```cpp
#include <iostream>
#include <vector>
using namespace std;

int main() {
    vector<int> numbers{1, 2, 3};

    for (int number : numbers) {
        cout << number << " ";
    }

    return 0;
}
```

This creates a vector containing three elements.

## Key Points

- `{}` provides consistent initialization syntax.
- Brace initialization works with built-in types, arrays, containers, and classes.
- It helps prevent narrowing conversions.
- Brace initialization can interact with `std::initializer_list` constructors.
