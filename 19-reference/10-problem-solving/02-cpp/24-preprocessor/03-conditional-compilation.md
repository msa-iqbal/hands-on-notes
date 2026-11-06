# Conditional Compilation

**Conditional compilation** allows parts of a program to be included or excluded before compilation.

Common directives include:

```text
#ifdef
#ifndef
#if
#elif
#else
#endif
```

## Example 1: `#ifdef`

```cpp
#include <iostream>
using namespace std;

#define DEBUG

int main() {
#ifdef DEBUG
    cout << "Debug mode" << endl;
#endif

    cout << "Program running" << endl;

    return 0;
}
```

### Output

```text
Debug mode
Program running
```

## Example 2: Without Definition

```cpp
#include <iostream>
using namespace std;

int main() {
#ifdef DEBUG
    cout << "Debug mode" << endl;
#endif

    cout << "Program running" << endl;

    return 0;
}
```

### Output

```text
Program running
```

## Example 3: `#ifndef`

`#ifndef` means "if not defined".

```cpp
#include <iostream>
using namespace std;

#ifndef VERSION
#define VERSION 1
#endif

int main() {
    cout << VERSION << endl;

    return 0;
}
```

### Output

```text
1
```

## Example 4: `#if`

```cpp
#include <iostream>
using namespace std;

#define VERSION 2

int main() {
#if VERSION == 1
    cout << "Version 1" << endl;
#elif VERSION == 2
    cout << "Version 2" << endl;
#else
    cout << "Unknown version" << endl;
#endif

    return 0;
}
```

### Output

```text
Version 2
```

## Example 5: `#else`

```cpp
#include <iostream>
using namespace std;

#define DEBUG

int main() {
#ifdef DEBUG
    cout << "Debug build" << endl;
#else
    cout << "Release build" << endl;
#endif

    return 0;
}
```

### Output

```text
Debug build
```

## Example 6: Platform-Specific Code

Predefined macros can be used for platform-specific compilation.

```cpp
#include <iostream>
using namespace std;

int main() {
#ifdef _WIN32
    cout << "Windows" << endl;
#elif defined(__linux__)
    cout << "Linux" << endl;
#elif defined(__APPLE__)
    cout << "Apple platform" << endl;
#else
    cout << "Unknown platform" << endl;
#endif

    return 0;
}
```

The actual output depends on the target platform and compiler.

## Example 7: Debug Logging

```cpp
#include <iostream>
using namespace std;

#ifdef DEBUG
#define LOG(message) cout << "[DEBUG] " << message << endl
#else
#define LOG(message)
#endif

int main() {
    LOG("Program started");

    cout << "Application running" << endl;

    return 0;
}
```

Compile with:

```bash
g++ -DDEBUG program.cpp -o program
```

Then debug logging is enabled.

Without `-DDEBUG`:

```bash
g++ program.cpp -o program
```

the `DEBUG` macro is not defined.

## Example 8: Header Guard

Conditional compilation is commonly used for header guards.

```cpp
#ifndef MY_HEADER_H
#define MY_HEADER_H

// Header declarations

#endif
```

## Key Points

|Directive|Meaning|
|---|---|
|`#ifdef`|If macro is defined|
|`#ifndef`|If macro is not defined|
|`#if`|If condition is true|
|`#elif`|Else-if condition|
|`#else`|Alternative branch|
|`#endif`|Ends conditional section|
|`defined()`|Checks whether a macro exists|

Conditional compilation is useful for:

- Debug/release builds
- Platform-specific code
- Compiler-specific code
- Feature flags
- Header guards
- Optional functionality
