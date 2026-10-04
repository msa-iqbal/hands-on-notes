# Predefined Macros

C++ compilers provide predefined macros that contain information about the source file, line number, date, time, and compiler environment.

## Example 1: `__FILE__`

`__FILE__` contains the current source file name.

```cpp
#include <iostream>
using namespace std;

int main() {
    cout << __FILE__ << endl;

    return 0;
}
```

Example output:

```text
main.cpp
```

The exact output depends on how the source file is compiled.

## Example 2: `__LINE__`

`__LINE__` contains the current source line number.

```cpp
#include <iostream>
using namespace std;

int main() {
    cout << __LINE__ << endl;

    return 0;
}
```

The output depends on where the statement appears in the file.

## Example 3: `__DATE__`

`__DATE__` contains the compilation date supplied by the implementation.

```cpp
#include <iostream>
using namespace std;

int main() {
    cout << "Build date: " << __DATE__ << endl;

    return 0;
}
```

## Example 4: `__TIME__`

`__TIME__` contains the compilation time supplied by the implementation.

```cpp
#include <iostream>
using namespace std;

int main() {
    cout << "Build time: " << __TIME__ << endl;

    return 0;
}
```

## Example 5: `__cplusplus`

`__cplusplus` identifies the C++ language standard mode used by the compiler.

```cpp
#include <iostream>
using namespace std;

int main() {
    cout << __cplusplus << endl;

    return 0;
}
```

Typical values include:

|Standard|Typical `__cplusplus` value|
|---|--:|
|C++11|`201103L`|
|C++14|`201402L`|
|C++17|`201703L`|
|C++20|`202002L`|
|C++23|`202302L`|

The exact behavior can depend on the compiler and language mode.

## Example 6: Check C++ Version

```cpp
#include <iostream>
using namespace std;

int main() {
#if __cplusplus >= 202002L
    cout << "C++20 or later" << endl;
#elif __cplusplus >= 201703L
    cout << "C++17" << endl;
#elif __cplusplus >= 201402L
    cout << "C++14" << endl;
#elif __cplusplus >= 201103L
    cout << "C++11" << endl;
#else
    cout << "Older C++ standard" << endl;
#endif

    return 0;
}
```

## Example 7: Debug Information

Predefined macros can be useful for debugging.

```cpp
#include <iostream>
using namespace std;

#define LOG(message) \
    cout << "[DEBUG] " \
         << __FILE__ << ":" \
         << __LINE__ << " - " \
         << message << endl;

int main() {
    LOG("Program started");

    return 0;
}
```

Example output:

```text
[DEBUG] main.cpp:12 - Program started
```

The exact line number depends on the source file.

## Example 8: Compiler-Specific Macros

Compilers may provide additional predefined macros.

For example:

```cpp
#include <iostream>
using namespace std;

int main() {
#ifdef __GNUC__
    cout << "GCC-compatible compiler" << endl;
#endif

#ifdef _MSC_VER
    cout << "Microsoft Visual C++ compiler" << endl;
#endif

#ifdef __clang__
    cout << "Clang compiler" << endl;
#endif

    return 0;
}
```

Which messages appear depends on the compiler.

## Common Predefined Macros

|Macro|Purpose|
|---|---|
|`__FILE__`|Current source file|
|`__LINE__`|Current source line|
|`__DATE__`|Compilation date|
|`__TIME__`|Compilation time|
|`__cplusplus`|C++ language standard indicator|
|`__func__`|Current function name in C++|
|`_WIN32`|Windows target indicator on common toolchains|
|`__linux__`|Linux target indicator on common toolchains|
|`__APPLE__`|Apple platform indicator on common toolchains|
|`__GNUC__`|GCC compatibility/compiler indicator|
|`_MSC_VER`|MSVC version indicator|
|`__clang__`|Clang indicator|

## `__func__`

Unlike most preprocessor macros, `__func__` is a predefined identifier provided by the C++ language.

```cpp
#include <iostream>
using namespace std;

void show() {
    cout << __func__ << endl;
}

int main() {
    show();

    return 0;
}
```

### Output

```text
show
```

## Key Points

- Predefined macros provide compile-time environment information.

- `__FILE__` identifies the source file.
- `__LINE__` identifies the source line.
- `__DATE__` and `__TIME__` provide build-time information.
- `__cplusplus` can be used to detect the language standard.
- Compiler/platform macros can support conditional compilation.
- Compiler-specific macros should be used carefully because they reduce portability.
