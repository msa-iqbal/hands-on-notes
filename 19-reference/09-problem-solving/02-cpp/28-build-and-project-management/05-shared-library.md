# Shared Library

> Learn how to create and use a shared C++ library.

### 1. What Is a Shared Library?

A shared library is loaded separately from the executable and can be shared by multiple processes.

On Linux, shared libraries commonly use:

```text
.so
```

Example:

```text
libcalculator.so
```

### 2. Project Structure

```text
project/
├── include/
│   └── calculator.h
├── src/
│   └── calculator.cpp
└── main.cpp
```

### 3. Header

```cpp
#ifndef CALCULATOR_H
#define CALCULATOR_H

int add(int a, int b);

#endif
```

### 4. Source

```cpp
#include "calculator.h"

int add(int a, int b)
{
    return a + b;
}
```

### 5. Compile with Position-Independent Code

```bash
g++ -std=c++17 \
    -fPIC \
    -Iinclude \
    -c src/calculator.cpp \
    -o calculator.o
```

`-fPIC` generates position-independent code, commonly used for shared libraries.

### 6. Create Shared Library

```bash
g++ -shared calculator.o -o libcalculator.so
```

Now:

```text
libcalculator.so
```

exists.

### 7. Main Program

```cpp
#include <iostream>
#include "calculator.h"

int main()
{
    std::cout << add(10, 20) << '\n';

    return 0;
}
```

### 8. Link the Shared Library

```bash
g++ -std=c++17 \
    main.cpp \
    -Iinclude \
    -L. \
    -lcalculator \
    -o app
```

### 9. Runtime Library Search

If `libcalculator.so` is in the current directory, Linux may not search the current directory automatically.

For a local test:

```bash
LD_LIBRARY_PATH=. ./app
```

Output:

```text
30
```

### 10. Use RPATH for a Local Example

You can embed a runtime search path:

```bash
g++ -std=c++17 \
    main.cpp \
    -Iinclude \
    -L. \
    -Wl,-rpath,'$ORIGIN' \
    -lcalculator \
    -o app
```

Then:

```bash
./app
```

can locate the library beside the executable.

### 11. Static vs Shared Library

|Feature|Static|Shared|
|---|---|---|
|Linux extension|`.a`|`.so`|
|Linked into executable|Yes|Dynamically loaded/linked|
|Separate runtime file|Usually no|Usually yes|
|Can be shared by processes|Not in the same runtime-library sense|Yes|
|Typical memory sharing|Less|Can allow shared library pages|
|Updating library independently|Usually rebuild application|Often possible with ABI compatibility|

### 12. Complete Example

Build library:

```bash
g++ -std=c++17 -fPIC \
    -Iinclude \
    -c src/calculator.cpp \
    -o calculator.o
```

Create:

```bash
g++ -shared calculator.o -o libcalculator.so
```

Build application:

```bash
g++ -std=c++17 \
    main.cpp \
    -Iinclude \
    -L. \
    -Wl,-rpath,'$ORIGIN' \
    -lcalculator \
    -o app
```

Run:

```bash
./app
```

### 13. Key Points

- Shared libraries commonly use `.so` on Linux.
- `-fPIC` is commonly used when compiling objects for shared libraries.
- `-shared` creates a shared library.
- `-L` specifies library search directories.
- `-lcalculator` searches for `libcalculator`.
- Runtime library discovery is separate from compile-time library discovery.
- ABI compatibility matters when replacing shared libraries independently.
