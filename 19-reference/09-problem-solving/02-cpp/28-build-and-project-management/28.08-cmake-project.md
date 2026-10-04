# CMake Project

> Build a practical multi-file C++ project using CMake.

### 1. Project Structure

```text
calculator-project/
├── CMakeLists.txt
│
├── include/
│   └── calculator.h
│
├── src/
│   ├── calculator.cpp
│   └── main.cpp
│
└── build/
```

The `build/` directory is generated and can be kept separate from source code.

### 2. Header

`include/calculator.h`

```cpp
#ifndef CALCULATOR_H
#define CALCULATOR_H

class Calculator
{
public:
    int add(int a, int b) const;
    int subtract(int a, int b) const;
    int multiply(int a, int b) const;
};

#endif
```

### 3. Implementation

`src/calculator.cpp`

```cpp
#include "calculator.h"

int Calculator::add(int a, int b) const
{
    return a + b;
}

int Calculator::subtract(int a, int b) const
{
    return a - b;
}

int Calculator::multiply(int a, int b) const
{
    return a * b;
}
```

### 4. Main

`src/main.cpp`

```cpp
#include <iostream>
#include "calculator.h"

int main()
{
    Calculator calculator;

    std::cout
        << "Add: "
        << calculator.add(10, 5)
        << '\n';

    std::cout
        << "Subtract: "
        << calculator.subtract(10, 5)
        << '\n';

    std::cout
        << "Multiply: "
        << calculator.multiply(10, 5)
        << '\n';

    return 0;
}
```

### 5. Basic CMake Configuration

`CMakeLists.txt`

```cmake
cmake_minimum_required(VERSION 3.16)

project(CalculatorProject)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

add_executable(
    calculator
    src/main.cpp
    src/calculator.cpp
)

target_include_directories(
    calculator
    PRIVATE
    ${CMAKE_CURRENT_SOURCE_DIR}/include
)
```

### 6. Configure

From the project root:

```bash
cmake -S . -B build
```

### 7. Build

```bash
cmake --build build
```

For a common single-config generator, the executable will typically be:

```text
build/calculator
```

Run:

```bash
./build/calculator
```

Output:

```text
Add: 15
Subtract: 5
Multiply: 50
```

### 8. Debug Build

Configure:

```bash
cmake -S . -B build-debug \
    -DCMAKE_BUILD_TYPE=Debug
```

Build:

```bash
cmake --build build-debug
```

### 9. Release Build

Configure:

```bash
cmake -S . -B build-release \
    -DCMAKE_BUILD_TYPE=Release
```

Build:

```bash
cmake --build build-release
```

### 10. Add Compiler Warnings

You can add target-specific warning options.

For GCC/Clang:

```cmake
target_compile_options(
    calculator
    PRIVATE
    -Wall
    -Wextra
    -Wpedantic
)
```

This keeps the options associated with the target.

### 11. Add More Source Files

Suppose:

```text
src/
├── main.cpp
├── calculator.cpp
├── logger.cpp
└── validator.cpp
```

CMake:

```cmake
add_executable(
    calculator
    src/main.cpp
    src/calculator.cpp
    src/logger.cpp
    src/validator.cpp
)
```

### 12. CMake Library Target

Instead of placing all implementation directly into an executable, create a library:

```cmake
add_library(
    calculator_lib
    src/calculator.cpp
)
```

Then:

```cmake
target_include_directories(
    calculator_lib
    PUBLIC
    ${CMAKE_CURRENT_SOURCE_DIR}/include
)
```

Create the executable:

```cmake
add_executable(
    calculator
    src/main.cpp
)
```

Link the library:

```cmake
target_link_libraries(
    calculator
    PRIVATE
    calculator_lib
)
```

### 13. Complete Library-Based CMakeLists

```cmake
cmake_minimum_required(VERSION 3.16)

project(CalculatorProject)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

add_library(
    calculator_lib
    src/calculator.cpp
)

target_include_directories(
    calculator_lib
    PUBLIC
    ${CMAKE_CURRENT_SOURCE_DIR}/include
)

target_compile_options(
    calculator_lib
    PRIVATE
    -Wall
    -Wextra
    -Wpedantic
)

add_executable(
    calculator
    src/main.cpp
)

target_link_libraries(
    calculator
    PRIVATE
    calculator_lib
)
```

### 14. Build

```bash
cmake -S . -B build
```

```bash
cmake --build build
```

Run:

```bash
./build/calculator
```

### 15. Installation Layout

A larger CMake project may define installation rules:

```cmake
include(GNUInstallDirs)

install(
    TARGETS calculator
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
)
```

Then:

```bash
cmake --install build
```

The exact installation location depends on the configured install prefix.

### 16. CMake Presets

Modern projects can use:

```text
CMakePresets.json
```

to store reusable configure/build presets.

Example:

```json
{
    "version": 6,
    "configurePresets": [
        {
            "name": "debug",
            "generator": "Unix Makefiles",
            "binaryDir": "${sourceDir}/build/debug",
            "cacheVariables": {
                "CMAKE_BUILD_TYPE": "Debug"
            }
        }
    ]
}
```

Then:

```bash
cmake --preset debug
```

Build:

```bash
cmake --build --preset debug
```

Preset schema/version support depends on the installed CMake version.

### 17. Recommended Workflow

Create:

```text
project/
├── CMakeLists.txt
├── include/
├── src/
└── build/
```

Configure:

```bash
cmake -S . -B build
```

Build:

```bash
cmake --build build
```

Run:

```bash
./build/calculator
```

After source changes:

```bash
cmake --build build
```

CMake/build-system dependency tracking determines what needs rebuilding.

### 18. Clean Build

If the build directory needs to be recreated:

```bash
rm -rf build
```

Then:

```bash
cmake -S . -B build
cmake --build build
```

Be careful with `rm -rf`; verify that the path is the intended generated build directory.

### 9. CMake vs Make

| Feature                           | Make               | CMake                      |
| --------------------------------- | ------------------ | -------------------------- |
| Direct build tool                 | Yes                | No, generator              |
| Uses Makefiles                    | Yes                | Can generate them          |
| Multi-platform project generation | Limited            | Strong                     |
| IDE/project generation            | Limited            | Yes                        |
| Dependency management             | Manual/build rules | Target-based configuration |
| Large project organization        | Possible           | Strong target model        |

CMake and Make are not direct equivalents: CMake commonly generates build-system files, while Make executes build rules.
### 20. Key Points

- `CMakeLists.txt` describes the project and its targets.
- `cmake -S . -B build` configures the build.
- `cmake --build build` builds it.
- `add_executable()` creates executable targets.
- `add_library()` creates library targets.
- `target_include_directories()` controls include directories.
- `target_link_libraries()` connects targets.
- `target_compile_options()` adds compiler options to a target.
- Out-of-source builds keep generated files separate from source files.
- CMake can generate build systems for different environments.
