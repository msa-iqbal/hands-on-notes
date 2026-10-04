# using namespace

The `using namespace` directive allows names from a namespace to be used without repeatedly writing the namespace name.

## Example 1: using namespace std

```cpp
#include <iostream>

using namespace std;

int main() {
    cout << "Hello C++" << endl;

    return 0;
}
```

Without it:

```cpp
#include <iostream>

int main() {
    std::cout << "Hello C++" << std::endl;

    return 0;
}
```

## Example 2: Custom Namespace

```cpp
#include <iostream>
using namespace std;

namespace Math {
    int add(int a, int b) {
        return a + b;
    }
}

using namespace Math;

int main() {
    cout << add(10, 20) << endl;

    return 0;
}
```

### Output

```text
30
```

## Example 3: using Declaration

Instead of importing the entire namespace, a specific name can be introduced.

```cpp
#include <iostream>

namespace Math {
    int add(int a, int b) {
        return a + b;
    }

    int subtract(int a, int b) {
        return a - b;
    }
}

using Math::add;

int main() {
    std::cout << add(10, 20) << std::endl;

    std::cout << Math::subtract(20, 10) << std::endl;

    return 0;
}
```

## Example 4: Avoiding Full Namespace Import

```cpp
#include <iostream>
#include <string>

using std::cout;
using std::endl;
using std::string;

int main() {
    string name = "Alice";

    cout << name << endl;

    return 0;
}
```

## Naming Conflict

Using an entire namespace can create ambiguity.

```cpp
#include <iostream>

namespace First {
    void show() {
        std::cout << "First" << std::endl;
    }
}

namespace Second {
    void show() {
        std::cout << "Second" << std::endl;
    }
}

int main() {
    First::show();
    Second::show();

    return 0;
}
```

Using both namespaces globally could make `show()` ambiguous.

## Recommendation

For small examples:

```cpp
using namespace std;
```

is common.

In larger projects, prefer:

```cpp
std::cout
std::string
std::vector
```

or specific declarations:

```cpp
using std::cout;
using std::string;
```

## Key Points

- `using namespace X;` imports names into the current scope.
- `using X::name;` imports a specific name.
- Avoid unnecessary global namespace imports in large projects.
- Explicit namespace qualification makes ownership clearer.
