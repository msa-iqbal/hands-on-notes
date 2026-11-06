# Custom Header File

A **custom header file** contains declarations that can be shared between multiple source files.

A common project structure is:

```text
project/
├── main.cpp
├── math.cpp
└── math.h
```

## Example 1: Header File

### `math.h`

```cpp
#ifndef MATH_H
#define MATH_H

int add(int a, int b);
int subtract(int a, int b);

#endif
```

### `math.cpp`

```cpp
#include "math.h"

int add(int a, int b) {
    return a + b;
}

int subtract(int a, int b) {
    return a - b;
}
```

### `main.cpp`

```cpp
#include <iostream>
#include "math.h"

int main() {
    std::cout << add(10, 20) << std::endl;
    std::cout << subtract(20, 10) << std::endl;

    return 0;
}
```

Compile:

```bash
g++ main.cpp math.cpp -o app
```

Run:

```bash
./app
```

### Output

```text
30
10
```

## Example 2: Header With Class

### `person.h`

```cpp
#ifndef PERSON_H
#define PERSON_H

#include <string>

class Person {
private:
    std::string name;
    int age;

public:
    Person(std::string name, int age);

    void show() const;
};

#endif
```

### `person.cpp`

```cpp
#include "person.h"
#include <iostream>
#include <utility>

Person::Person(std::string name, int age)
    : name(std::move(name)),
      age(age) {}

void Person::show() const {
    std::cout << name
              << " "
              << age
              << std::endl;
}
```

### `main.cpp`

```cpp
#include "person.h"

int main() {
    Person person("Alice", 25);

    person.show();

    return 0;
}
```

Compile:

```bash
g++ main.cpp person.cpp -o app
```

## Declaration vs Definition

Header:

```cpp
int add(int a, int b);
```

This is a declaration.

Source file:

```cpp
int add(int a, int b) {
    return a + b;
}
```

This is a definition.

## Key Points

- Header files commonly contain declarations.
- Source files commonly contain definitions.
- Include custom headers using `"header.h"`.
- Separating interface and implementation improves project organization.
