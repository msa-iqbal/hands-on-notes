# Multiple Source Files

> Learn how to compile a C++ project containing multiple `.cpp` files.

### 1. Why Use Multiple Source Files?

Large programs should not normally put all code into one file.

A project can be divided into logical source files:

```text
project/
├── main.cpp
├── calculator.cpp
└── calculator.h
```

This makes code easier to organize, maintain, and reuse.

### 2. Simple Multiple `.cpp` Files

Directory:

```text
project/
├── main.cpp
├── add.cpp
└── subtract.cpp
```

###### `add.cpp`

```cpp
int add(int a, int b)
{
    return a + b;
}
```

###### `subtract.cpp`

```cpp
int subtract(int a, int b)
{
    return a - b;
}
```

###### `main.cpp`

```cpp
#include <iostream>

int add(int a, int b);
int subtract(int a, int b);

int main()
{
    std::cout << add(10, 5) << '\n';
    std::cout << subtract(10, 5) << '\n';

    return 0;
}
```

Compile all files:

```bash
g++ -std=c++17 main.cpp add.cpp subtract.cpp -o app
```

Run:

```bash
./app
```

Output:

```text
15
5
```

### 3. Use Header Files

A better structure is:

```text
project/
├── main.cpp
├── calculator.cpp
└── calculator.h
```

###### `calculator.h`

```cpp
#ifndef CALCULATOR_H
#define CALCULATOR_H

int add(int a, int b);
int subtract(int a, int b);

#endif
```

###### `calculator.cpp`

```cpp
#include "calculator.h"

int add(int a, int b)
{
    return a + b;
}

int subtract(int a, int b)
{
    return a - b;
}
```

###### `main.cpp`

```cpp
#include <iostream>
#include "calculator.h"

int main()
{
    std::cout << add(10, 5) << '\n';
    std::cout << subtract(10, 5) << '\n';

    return 0;
}
```

Compile:

```bash
g++ -std=c++17 main.cpp calculator.cpp -o app
```

### 4. Compile Files Separately

Compile:

```bash
g++ -std=c++17 -c main.cpp -o main.o
```

```bash
g++ -std=c++17 -c calculator.cpp -o calculator.o
```

Link:

```bash
g++ main.o calculator.o -o app
```

Run:

```bash
./app
```

### 5. Three Source Files

Directory:

```text
project/
├── main.cpp
├── math.cpp
├── string_utils.cpp
└── include/
    ├── math.h
    └── string_utils.h
```

###### `math.h`

```cpp
#ifndef MATH_H
#define MATH_H

int add(int a, int b);
int multiply(int a, int b);

#endif
```

###### `math.cpp`

```cpp
#include "math.h"

int add(int a, int b)
{
    return a + b;
}

int multiply(int a, int b)
{
    return a * b;
}
```

###### `string_utils.h`

```cpp
#ifndef STRING_UTILS_H
#define STRING_UTILS_H

bool is_empty(const char* text);

#endif
```

###### `string_utils.cpp`

```cpp
#include "string_utils.h"

bool is_empty(const char* text)
{
    return text == nullptr || text[0] == '\0';
}
```

###### `main.cpp`

```cpp
#include <iostream>

#include "math.h"
#include "string_utils.h"

int main()
{
    std::cout << add(2, 3) << '\n';
    std::cout << multiply(4, 5) << '\n';

    std::cout << std::boolalpha
              << is_empty("")
              << '\n';

    return 0;
}
```

Compile:

```bash
g++ -std=c++17 main.cpp math.cpp string_utils.cpp -o app
```

### 6. Compile All `.cpp` Files

For a small project:

```bash
g++ -std=c++17 *.cpp -o app
```

Then:

```bash
./app
```

Be careful with `*.cpp` if the directory contains source files that should not be part of the same executable.

### 7. Separate Compilation

Each source file can be compiled independently:

```text
main.cpp
   ↓
main.o

calculator.cpp
   ↓
calculator.o

logger.cpp
   ↓
logger.o

main.o + calculator.o + logger.o
              ↓
            linker
              ↓
             app
```

This is important for larger projects because changing one source file does not necessarily require compiling every source file again when using an incremental build system.

### 8. Undefined Reference Example

Suppose `main.cpp` contains:

```cpp
int add(int a, int b);

int main()
{
    return add(1, 2);
}
```

But you compile only:

```bash
g++ main.cpp -o app
```

If the definition of `add()` is in another source file and that file is not linked, the linker will report an undefined-reference error.

Correct:

```bash
g++ main.cpp calculator.cpp -o app
```

### 9. Header vs Source File

A common pattern:

```text
calculator.h
    ↓
Declarations

calculator.cpp
    ↓
Definitions

main.cpp
    ↓
Uses declarations
```

Example:

```cpp
// calculator.h
int add(int a, int b);
```

```cpp
// calculator.cpp
int add(int a, int b)
{
    return a + b;
}
```

```cpp
// main.cpp
#include "calculator.h"
```

### 10. Multiple Source Files with Classes

###### `person.h`

```cpp
#ifndef PERSON_H
#define PERSON_H

#include <string>

class Person
{
private:
    std::string name;

public:
    Person(const std::string& name);

    void greet() const;
};

#endif
```

###### `person.cpp`

```cpp
#include <iostream>
#include "person.h"

Person::Person(const std::string& name)
    : name(name)
{
}

void Person::greet() const
{
    std::cout << "Hello, "
              << name
              << '\n';
}
```

###### `main.cpp`

```cpp
#include "person.h"

int main()
{
    Person person("Alice");

    person.greet();

    return 0;
}
```

Compile:

```bash
g++ -std=c++17 main.cpp person.cpp -o app
```

Output:

```text
Hello, Alice
```

### 11. Key Points

- Multiple source files divide a project into manageable components.
- `.h` files commonly contain declarations.
- `.cpp` files commonly contain definitions.
- All required `.cpp` files must be compiled and linked.
- `-c` creates object files.
- The linker combines object files into an executable.
- Header guards prevent repeated header inclusion.
- Build systems such as Make and CMake automate this process.
