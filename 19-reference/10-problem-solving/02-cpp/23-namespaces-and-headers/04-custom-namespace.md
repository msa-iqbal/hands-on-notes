# Custom Namespace

A **custom namespace** is a namespace created by the programmer to organize application-specific code.

## Example 1: Utility Namespace

```cpp
#include <iostream>
using namespace std;

namespace Utilities {
    void printMessage() {
        cout << "Hello from Utilities" << endl;
    }

    int square(int number) {
        return number * number;
    }
}

int main() {
    Utilities::printMessage();

    cout << Utilities::square(5) << endl;

    return 0;
}
```

### Output

```text
Hello from Utilities
25
```

## Example 2: Application Namespace

```cpp
#include <iostream>
#include <string>
using namespace std;

namespace MyApp {
    string applicationName = "Student Management";

    void start() {
        cout << applicationName << endl;
    }
}

int main() {
    MyApp::start();

    return 0;
}
```

## Example 3: Namespace With Classes

```cpp
#include <iostream>
#include <string>
using namespace std;

namespace Banking {
    class Account {
    private:
        string owner;
        double balance;

    public:
        Account(string owner, double balance)
            : owner(owner), balance(balance) {}

        void show() const {
            cout << owner << ": "
                 << balance << endl;
        }
    };
}

int main() {
    Banking::Account account("Alice", 5000);

    account.show();

    return 0;
}
```

### Output

```text
Alice: 5000
```

## Example 4: Namespace Alias

A long namespace can be given a shorter alias.

```cpp
#include <iostream>
using namespace std;

namespace Application::Database::PostgreSQL {
    void connect() {
        cout << "Connected" << endl;
    }
}

namespace DB = Application::Database::PostgreSQL;

int main() {
    DB::connect();

    return 0;
}
```

### Output

```text
Connected
```

## Example 5: Namespace Across Files

### `math.h`

```cpp
#ifndef MATH_H
#define MATH_H

namespace Math {
    int add(int a, int b);
}

#endif
```

### `math.cpp`

```cpp
#include "math.h"

namespace Math {
    int add(int a, int b) {
        return a + b;
    }
}
```

### `main.cpp`

```cpp
#include <iostream>
#include "math.h"

int main() {
    std::cout << Math::add(10, 20) << std::endl;

    return 0;
}
```

## Key Points

- Custom namespaces separate application components.
- They reduce naming conflicts.
- Namespaces can contain classes, functions, variables, and types.
- Namespace aliases simplify long namespace names.
- A namespace can span multiple source files.
