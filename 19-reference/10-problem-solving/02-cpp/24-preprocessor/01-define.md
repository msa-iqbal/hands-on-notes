# `#define`

The `#define` directive is a C++ preprocessor directive used to define macros and symbolic constants.

Preprocessor directives are processed before the C++ compiler compiles the source code.

## Basic Syntax

```cpp
#define NAME value
```

## Example 1: Define a Constant

```cpp
#include <iostream>
using namespace std;

#define PI 3.14159

int main() {
    cout << PI << endl;

    return 0;
}
```

### Output

```text
3.14159
```

## Example 2: Integer Constant

```cpp
#include <iostream>
using namespace std;

#define MAX_USERS 100

int main() {
    cout << MAX_USERS << endl;

    return 0;
}
```

### Output

```text
100
```

## Example 3: String Constant

```cpp
#include <iostream>
using namespace std;

#define APP_NAME "Student Management System"

int main() {
    cout << APP_NAME << endl;

    return 0;
}
```

### Output

```text
Student Management System
```

## Example 4: Multiple Definitions

```cpp
#include <iostream>
using namespace std;

#define WIDTH 10
#define HEIGHT 20

int main() {
    int area = WIDTH * HEIGHT;

    cout << "Area: " << area << endl;

    return 0;
}
```

### Output

```text
Area: 200
```

## Example 5: `#undef`

A macro can be removed using `#undef`.

```cpp
#include <iostream>
using namespace std;

#define VALUE 100

#undef VALUE

int main() {
    // cout << VALUE; // Error: VALUE is no longer defined

    cout << "VALUE was undefined" << endl;

    return 0;
}
```

### Output

```text
VALUE was undefined
```

## Example 6: Conditional Definition

```cpp
#include <iostream>
using namespace std;

#define DEBUG

int main() {
#ifdef DEBUG
    cout << "Debug mode enabled" << endl;
#endif

    return 0;
}
```

### Output

```text
Debug mode enabled
```

## `#define` vs `const`

Modern C++ generally prefers typed constants when possible.

```cpp
const int maxUsers = 100;
```

or:

```cpp
constexpr int maxUsers = 100;
```

Compared with:

```cpp
#define MAX_USERS 100
```

`const` and `constexpr` provide type information and follow C++ scope and language rules.

## Example 7: constexpr Alternative

```cpp
#include <iostream>
using namespace std;

constexpr int MAX_USERS = 100;

int main() {
    cout << MAX_USERS << endl;

    return 0;
}
```

## Key Points

- `#define` is processed by the preprocessor.
- It can define symbolic constants.
- `#undef` removes a macro definition.
- Macros do not have normal C++ type information.
- Prefer `const` or `constexpr` for ordinary constants when possible.
