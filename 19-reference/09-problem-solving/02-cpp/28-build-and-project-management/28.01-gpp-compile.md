# g++ Compile

> Learn how to compile and run C++ programs using `g++`.

### 1. What Is `g++`?

`g++` is the GNU C++ compiler driver provided by GCC.

Basic command:

```bash
g++ main.cpp -o main
```

This:

1. Compiles `main.cpp`.

2. Produces an executable named `main`.

Run:

```bash
./main
```

### 2. Basic C++ Program

Create `main.cpp`:

```cpp
#include <iostream>

using namespace std;

int main()
{
    cout << "Hello, C++!\n";

    return 0;
}
```

Compile:

```bash
g++ main.cpp -o main
```

Run:

```bash
./main
```

Output:

```text
Hello, C++!
```

### 3. Specify C++ Standard

Use:

```bash
g++ -std=c++17 main.cpp -o main
```

Common standards:

```bash
g++ -std=c++11 main.cpp -o main
g++ -std=c++14 main.cpp -o main
g++ -std=c++17 main.cpp -o main
g++ -std=c++20 main.cpp -o main
g++ -std=c++23 main.cpp -o main
```

For this repository, C++17 is a practical default unless a topic requires a newer standard.

### 4. Compile Without `-o`

```bash
g++ main.cpp
```

By default, GCC typically creates:

```text
a.out
```

Run:

```bash
./a.out
```

Using `-o` is usually clearer:

```bash
g++ main.cpp -o main
```

### 5. Enable Warnings

Use:

```bash
g++ -std=c++17 -Wall main.cpp -o main
```

More warnings:

```bash
g++ -std=c++17 -Wall -Wextra main.cpp -o main
```

A useful development command:

```bash
g++ -std=c++17 -Wall -Wextra -pedantic main.cpp -o main
```

### 6. Debug Build

Use debug information:

```bash
g++ -std=c++17 -Wall -Wextra -g main.cpp -o main
```

The `-g` option adds debugging information useful with debuggers such as GDB.

### 7. Optimization

Common optimization levels include:

```bash
-O0
-O1
-O2
-O3
```

Example:

```bash
g++ -std=c++17 -O2 main.cpp -o main
```

For learning and debugging, a simple build such as:

```bash
g++ -std=c++17 -Wall -Wextra -g main.cpp -o main
```

is often convenient.

### 8. Compile and Run in One Command

Linux/macOS shell:

```bash
g++ -std=c++17 main.cpp -o main && ./main
```

If compilation fails, `./main` will not execute because `&&` requires the previous command to succeed.

### 9. Source File and Executable

```text
main.cpp
   │
   │ g++
   ▼
main
   │
   │ execute
   ▼
Program
```

### 10. Show Compilation Errors

Suppose:

```cpp
#include <iostream>

int main()
{
    std::cout << "Hello\n"

    return 0;
}
```

Compile:

```bash
g++ -std=c++17 main.cpp -o main
```

The compiler reports a syntax error because the output statement is missing `;`.

Compiler diagnostics normally include useful information such as:

```text
filename
line number
column/location
error message
```

### 11. Object File Compilation

You can compile a source file into an object file:

```bash
g++ -std=c++17 -c main.cpp -o main.o
```

This produces:

```text
main.o
```

Then link it:

```bash
g++ main.o -o main
```

Run:

```bash
./main
```

### 12. Compile and Link

Conceptually:

```text
main.cpp
   │
   ▼
compiler
   │
   ▼
main.o
   │
   ▼
linker
   │
   ▼
main
```

For a simple program, `g++ main.cpp -o main` performs the necessary compilation and linking automatically.

### 13. Useful Compiler Commands

###### Basic

```bash
g++ main.cpp -o main
```

###### C++17

```bash
g++ -std=c++17 main.cpp -o main
```

###### Warnings

```bash
g++ -std=c++17 -Wall -Wextra main.cpp -o main
```

###### Debug

```bash
g++ -std=c++17 -Wall -Wextra -g main.cpp -o main
```

###### Optimization

```bash
g++ -std=c++17 -O2 main.cpp -o main
```

###### Object file

```bash
g++ -std=c++17 -c main.cpp -o main.o
```

### 14. Check Compiler Version

```bash
g++ --version
```

Example:

```text
g++ (GCC) ...
```

The exact version depends on the installed GCC package.

### 15. Key Points

- `g++` compiles C++ source code.
- Use `-std=c++17` to select C++17.
- Use `-o` to specify the executable name.
- `-Wall -Wextra` enables useful compiler warnings.
- `-g` adds debugging information.
- `-O2` enables optimization.
- `-c` produces an object file without linking.
- `g++ source.cpp -o program` is the basic compilation pattern.
