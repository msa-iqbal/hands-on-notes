# Header and Source Files

> Learn how to organize declarations and definitions using `.h` and `.cpp` files.

### 1. Header File

A header file commonly contains declarations that other source files need.

Example:

```cpp
// calculator.h

int add(int a, int b);
```

The header tells the compiler that `add()` exists.

### 2. Source File

The source file contains the implementation.

```cpp
// calculator.cpp

#include "calculator.h"

int add(int a, int b)
{
    return a + b;
}
```

### 3. Main Source File

```cpp
// main.cpp

#include <iostream>
#include "calculator.h"

int main()
{
    std::cout << add(10, 20) << '\n';

    return 0;
}
```

Compile:

```bash
g++ -std=c++17 main.cpp calculator.cpp -o app
```

Run:

```bash
./app
```

Output:

```text
30
```

### 4. Declaration vs Definition

###### Declaration

```cpp
int add(int a, int b);
```

This tells the compiler about the function.

###### Definition

```cpp
int add(int a, int b)
{
    return a + b;
}
```

This provides the implementation.

### 5. Header Guards

A header can be protected with an include guard:

```cpp
#ifndef CALCULATOR_H
#define CALCULATOR_H

int add(int a, int b);

#endif
```

This prevents the header's contents from being processed more than once in the same translation unit.

### 6. `#pragma once`

Many compilers support:

```cpp
#pragma once

int add(int a, int b);
```

This is a convenient alternative to traditional include guards.

For portable textbook-style code, traditional include guards are still useful to understand.

### 7. Class in Header

###### `person.h`

```cpp
#ifndef PERSON_H
#define PERSON_H

#include <string>

class Person
{
private:
    std::string name;
    int age;

public:
    Person(
        const std::string& name,
        int age
    );

    void display() const;
};

#endif
```

### 8. Class Implementation in Source

###### `person.cpp`

```cpp
#include <iostream>
#include "person.h"

Person::Person(
    const std::string& name,
    int age
)
    : name(name),
      age(age)
{
}

void Person::display() const
{
    std::cout
        << "Name: " << name << '\n'
        << "Age: " << age << '\n';
}
```

### 9. Use the Class

###### `main.cpp`

```cpp
#include "person.h"

int main()
{
    Person person("Alice", 25);

    person.display();

    return 0;
}
```

Compile:

```bash
g++ -std=c++17 main.cpp person.cpp -o app
```

Output:

```text
Name: Alice
Age: 25
```

### 10. Directory Structure

A common project structure:

```text
project/
├── include/
│   └── calculator.h
│
├── src/
│   └── calculator.cpp
│
└── main.cpp
```

Compile:

```bash
g++ -std=c++17 \
    main.cpp \
    src/calculator.cpp \
    -Iinclude \
    -o app
```

The `-Iinclude` option adds `include/` to the header search path.

### 11. Better Project Structure

For a larger project:

```text
project/
├── include/
│   ├── calculator.h
│   ├── person.h
│   └── logger.h
│
├── src/
│   ├── calculator.cpp
│   ├── person.cpp
│   └── logger.cpp
│
└── main.cpp
```

Compile:

```bash
g++ -std=c++17 \
    main.cpp \
    src/calculator.cpp \
    src/person.cpp \
    src/logger.cpp \
    -Iinclude \
    -o app
```

### 12. Header Include Styles

For standard library headers:

```cpp
#include <iostream>
#include <string>
#include <vector>
```

For project headers:

```cpp
#include "calculator.h"
#include "person.h"
```

The exact search behavior depends on the compiler options and include paths, but this convention is widely used.

### 13. Header with Constants

```cpp
#ifndef CONFIG_H
#define CONFIG_H

constexpr int MAX_USERS = 100;

#endif
```

Use:

```cpp
#include "config.h"

int main()
{
    return MAX_USERS;
}
```

For larger projects, avoid putting mutable global state directly in headers.

### 14. Header with Struct

###### `student.h`

```cpp
#ifndef STUDENT_H
#define STUDENT_H

#include <string>

struct Student
{
    std::string name;
    int age;
};

#endif
```

Use:

```cpp
#include <iostream>
#include "student.h"

int main()
{
    Student student{"Alice", 20};

    std::cout << student.name << '\n';
    std::cout << student.age << '\n';

    return 0;
}
```

### 15. Header with Function Declarations

```cpp
#ifndef MATH_UTILS_H
#define MATH_UTILS_H

int add(int a, int b);
int subtract(int a, int b);
int multiply(int a, int b);
double divide(double a, double b);

#endif
```

Implementation:

```cpp
#include "math_utils.h"

int add(int a, int b)
{
    return a + b;
}

int subtract(int a, int b)
{
    return a - b;
}

int multiply(int a, int b)
{
    return a * b;
}

double divide(double a, double b)
{
    return a / b;
}
```

### 16. Header with Inline Function

Small functions can be defined inline in a header:

```cpp
#ifndef MATH_UTILS_H
#define MATH_UTILS_H

inline int square(int value)
{
    return value * value;
}

#endif
```

Then:

```cpp
#include <iostream>
#include "math_utils.h"

int main()
{
    std::cout << square(5) << '\n';

    return 0;
}
```

Output:

```text
25
```

The `inline` keyword has language/linkage implications; it does not simply mean "force the compiler to inline this function."

### 17. Avoid Definitions That Cause Multiple Definitions

Do not normally put an ordinary non-inline function definition in a header included by multiple source files:

```cpp
// bad.h

int add(int a, int b)
{
    return a + b;
}
```

If several translation units include this header, it can lead to multiple-definition linker errors.

Instead:

```cpp
// good.h
int add(int a, int b);
```

and:

```cpp
// good.cpp
int add(int a, int b)
{
    return a + b;
}
```

Alternatively, appropriately defined `inline` functions can be placed in headers.

### 18. Translation Unit

When a `.cpp` file is compiled, its source plus included headers form a **translation unit**.

Conceptually:

```text
main.cpp
   +
calculator.h
   +
other included headers
   ↓
Translation Unit
   ↓
Compiler
   ↓
main.o
```

### 19. Compile Example

Project:

```text
project/
├── include/
│   └── calculator.h
├── src/
│   └── calculator.cpp
└── main.cpp
```

Command:

```bash
g++ -std=c++17 \
    main.cpp \
    src/calculator.cpp \
    -Iinclude \
    -o app
```

Run:

```bash
./app
```

### 20. Recommended Separation

A practical separation is:

```text
Header
├── declarations
├── class definitions
├── type definitions
└── constants/templates where appropriate

Source
├── function definitions
├── class member implementations
└── implementation details
```

This is a convention rather than an absolute language rule; templates and some modern C++ constructs often require definitions in headers.

### 21. Key Points

- Header files commonly expose declarations and interfaces.
- Source files commonly contain implementations.
- Use include guards or `#pragma once`.
- Use `#include <...>` commonly for standard/system headers.
- Use `#include "..."` commonly for project headers.
- `-I` adds custom header search directories.
- Header files are included into translation units before compilation.
- Avoid ordinary non-inline function definitions in headers included by multiple source files.
- Templates often need their definitions visible in headers.
- Separating interface and implementation improves project organization.
