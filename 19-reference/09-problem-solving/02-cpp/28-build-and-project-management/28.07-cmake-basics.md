# CMake Basics

> Learn the basic concepts and commands of CMake.

### 1. What Is CMake?

CMake is a build-system generator.

CMake does not directly replace the compiler. Instead, it generates build files for tools such as:

- Make
- Ninja
- Visual Studio
- Xcode

### 2. Basic Project

Structure:

```text
project/
├── CMakeLists.txt
└── main.cpp
```

`main.cpp`:

```cpp
#include <iostream>

int main()
{
    std::cout << "Hello CMake!\n";

    return 0;
}
```

### 3. Basic `CMakeLists.txt`

```cmake
cmake_minimum_required(VERSION 3.16)

project(MyApp)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

add_executable(app main.cpp)
```

### 4. Configure the Project

Create a build directory:

```bash
mkdir build
```

Enter it:

```bash
cd build
```

Configure:

```bash
cmake ..
```

CMake examines the project and generates build files.

### 5. Build the Project

From the build directory:

```bash
cmake --build .
```

Depending on the generator, the output executable will be placed in an appropriate build location.

Run it according to the generated build layout.

For a common single-config Makefile build:

```bash
./app
```

### 6. Out-of-Source Build

Recommended structure:

```text
project/
├── CMakeLists.txt
├── main.cpp
└── build/
```

Commands:

```bash
cmake -S . -B build
```

Build:

```bash
cmake --build build
```

This keeps generated build files separate from source files.

### 7. Specify C++ Standard

Modern CMake can use:

```cmake
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)
```

This requests standard C++17 rather than compiler-specific language extensions where supported.

### 8. Multiple Source Files

Structure:

```text
project/
├── CMakeLists.txt
├── main.cpp
├── calculator.cpp
└── calculator.h
```

CMake:

```cmake
cmake_minimum_required(VERSION 3.16)

project(Calculator)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

add_executable(
    app
    main.cpp
    calculator.cpp
)
```

Build:

```bash
cmake -S . -B build
cmake --build build
```

### 9. CMake Variables

Example:

```cmake
set(APP_NAME calculator)

add_executable(${APP_NAME} main.cpp)
```

This creates an executable target named:

```text
calculator
```

### 10. Build Type

For single-config generators:

```bash
cmake -S . -B build -DCMAKE_BUILD_TYPE=Debug
```

or:

```bash
cmake -S . -B build -DCMAKE_BUILD_TYPE=Release
```

For multi-config generators, the configuration is typically selected during the build command.

### 11. Common Build Commands

Configure:

```bash
cmake -S . -B build
```

Build:

```bash
cmake --build build
```

Clean/rebuild behavior depends on the generator, but a clean build directory can always be recreated when necessary.

### 12. CMake Project Flow

```text
CMakeLists.txt
      │
      ▼
    CMake
      │
      ▼
Build-system files
      │
      ▼
Compiler / Build tool
      │
      ▼
Executable
```

### 13. Key Points

- CMake is a build-system generator.
- `CMakeLists.txt` describes the project.
- `cmake -S . -B build` configures an out-of-source build.
- `cmake --build build` builds the project.
- `add_executable()` creates an executable target.
- CMake can generate files for multiple build systems and IDEs.
