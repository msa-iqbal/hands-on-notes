# Static Library

> Learn how to create and link a static C++ library.

### 1. What Is a Static Library?

A static library is a collection of object files packaged into an archive.

On Linux, a static library commonly has:

```text
libname.a
```

Example:

```text
libcalculator.a
```

At link time, the required library code is incorporated into the executable.

### 2. Project Structure

```text
project/
├── include/
│   └── calculator.h
├── src/
│   └── calculator.cpp
└── main.cpp
```

### 3. Header File

`include/calculator.h`

```cpp
#ifndef CALCULATOR_H
#define CALCULATOR_H

int add(int a, int b);
int multiply(int a, int b);

#endif
```

### 4. Source File

`src/calculator.cpp`

```cpp
#include "calculator.h"

int add(int a, int b)
{
    return a + b;
}

int multiply(int a, int b)
{
    return a * b;
}
```

### 5. Compile to Object File

```bash
g++ -std=c++17 -Iinclude -c src/calculator.cpp -o calculator.o
```

Now:

```text
calculator.o
```

exists.

### 6. Create Static Library

Use `ar`:

```bash
ar rcs libcalculator.a calculator.o
```

Now:

```text
libcalculator.a
```

exists.

### 7. Main Program

`main.cpp`

```cpp
#include <iostream>
#include "calculator.h"

int main()
{
    std::cout << add(10, 20) << '\n';
    std::cout << multiply(5, 6) << '\n';

    return 0;
}
```

### 8. Link the Static Library

```bash
g++ -std=c++17 \
    main.cpp \
    -Iinclude \
    -L. \
    -lcalculator \
    -o app
```

Meaning:

```text
-Iinclude
    Header search directory

-L.
    Library search directory

-lcalculator
    Search for libcalculator.a
```

Run:

```bash
./app
```

Output:

```text
30
30
```

### 9. Complete Build Sequence

```bash
g++ -std=c++17 -Iinclude -c src/calculator.cpp -o calculator.o
```

```bash
ar rcs libcalculator.a calculator.o
```

```bash
g++ -std=c++17 main.cpp -Iinclude -L. -lcalculator -o app
```

```bash
./app
```

### 10. Multiple Object Files

Suppose:

```text
math.o
string_utils.o
logger.o
```

Create:

```bash
ar rcs libutils.a math.o string_utils.o logger.o
```

Then link:

```bash
g++ main.o -L. -lutils -o app
```

### 11. Static Library Structure

```text
calculator.cpp
      │
      ▼
calculator.o
      │
      ▼
libcalculator.a
      │
      ▼
main.cpp + library
      │
      ▼
app
```

### 12. Advantages

Static libraries:

- Are easy to distribute with the application.

- Do not require a separate shared-library file at runtime in the usual case.

- Can simplify deployment.

- Allow reusable compiled components.

### 13. Key Points

- Static libraries on Linux commonly use `.a`.

- `ar rcs` creates an archive.

- `-L` specifies library directories.

- `-lNAME` searches for `libNAME.a` or an appropriate shared library.

- Object files are created with `g++ -c`.

- Static library code is linked into the executable as needed.




