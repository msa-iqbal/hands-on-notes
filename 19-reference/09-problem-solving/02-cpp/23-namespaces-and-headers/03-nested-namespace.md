# Nested Namespace

A **nested namespace** is a namespace declared inside another namespace.

## Example 1: Basic Nested Namespace

```cpp
#include <iostream>
using namespace std;

namespace Company {
    namespace HR {
        void show() {
            cout << "Human Resources" << endl;
        }
    }
}

int main() {
    Company::HR::show();

    return 0;
}
```

### Output

```text
Human Resources
```

## Example 2: C++17 Nested Namespace Syntax

C++17 allows nested namespaces to be written more compactly.

```cpp
#include <iostream>
using namespace std;

namespace Company::HR {
    void show() {
        cout << "Human Resources" << endl;
    }
}

int main() {
    Company::HR::show();

    return 0;
}
```

## Example 3: Multiple Levels

```cpp
#include <iostream>
using namespace std;

namespace Application::Database::PostgreSQL {
    void connect() {
        cout << "Connecting to PostgreSQL" << endl;
    }
}

int main() {
    Application::Database::PostgreSQL::connect();

    return 0;
}
```

### Output

```text
Connecting to PostgreSQL
```

## Example 4: Nested Classes and Namespace

```cpp
#include <iostream>
using namespace std;

namespace Company {
    namespace Development {
        class Developer {
        public:
            void show() {
                cout << "Developer" << endl;
            }
        };
    }
}

int main() {
    Company::Development::Developer developer;

    developer.show();

    return 0;
}
```

## Example 5: using Nested Namespace

```cpp
#include <iostream>
using namespace std;

namespace Application::Utilities {
    void print() {
        cout << "Utility function" << endl;
    }
}

using namespace Application::Utilities;

int main() {
    print();

    return 0;
}
```

## Key Points

- Namespaces can be nested.
- C++17 introduced compact nested namespace syntax.
- Nested namespaces are useful for organizing larger projects.
- Access nested members with multiple `::` operators.
