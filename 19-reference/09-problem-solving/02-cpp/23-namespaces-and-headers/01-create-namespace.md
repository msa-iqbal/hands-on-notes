# Create Namespace

A **namespace** is used to organize related identifiers and prevent naming conflicts.

## Basic Syntax

```cpp
namespace NamespaceName {
    // declarations
}
```

## Example 1: Basic Namespace

```cpp
#include <iostream>
using namespace std;

namespace Math {
    int add(int a, int b) {
        return a + b;
    }
}

int main() {
    cout << Math::add(10, 20) << endl;

    return 0;
}
```

### Output

```text
30
```

## Example 2: Namespace Variable

```cpp
#include <iostream>
using namespace std;

namespace Config {
    const int maxUsers = 100;
}

int main() {
    cout << Config::maxUsers << endl;

    return 0;
}
```

### Output

```text
100
```

## Example 3: Multiple Functions

```cpp
#include <iostream>
using namespace std;

namespace Calculator {
    int add(int a, int b) {
        return a + b;
    }

    int subtract(int a, int b) {
        return a - b;
    }

    int multiply(int a, int b) {
        return a * b;
    }
}

int main() {
    cout << Calculator::add(10, 5) << endl;
    cout << Calculator::subtract(10, 5) << endl;
    cout << Calculator::multiply(10, 5) << endl;

    return 0;
}
```

### Output

```text
15
5
50
```

## Example 4: Namespace With Class

```cpp
#include <iostream>
using namespace std;

namespace School {
    class Student {
    public:
        void show() {
            cout << "Student class" << endl;
        }
    };
}

int main() {
    School::Student student;

    student.show();

    return 0;
}
```

## Scope Resolution Operator

The `::` operator is used to access members of a namespace.

```cpp
Math::add();
School::Student;
Config::maxUsers;
```

## Key Points

- Namespaces organize code.
- They prevent naming conflicts.
- Namespace members are accessed using `::`.
- A namespace can contain variables, functions, classes, and other declarations.
