# Makefile

> Learn how to automate C++ builds using GNU Make.

### 1. What Is Make?

`make` is a build automation tool.

Instead of repeatedly typing:

```bash
g++ main.cpp calculator.cpp -o app
```

you can define the build rules in a:

```text
Makefile
```

and run:

```bash
make
```

### 2. Simple Makefile

Project:

```text
project/
├── main.cpp
├── calculator.cpp
├── calculator.h
└── Makefile
```

Makefile:

```makefile
CXX = g++
CXXFLAGS = -std=c++17 -Wall -Wextra

app: main.cpp calculator.cpp
 $(CXX) $(CXXFLAGS) main.cpp calculator.cpp -o app

clean:
 rm -f app
```

Run:

```bash
make
```

Then:

```bash
./app
```

### 3. Important Makefile Syntax

A rule has:

```text
target: prerequisites
<TAB> command
```

Example:

```makefile
app: main.cpp calculator.cpp
 g++ main.cpp calculator.cpp -o app
```

The command line must begin with a **tab** in a traditional Makefile.

### 4. Variables

```makefile
CXX = g++
CXXFLAGS = -std=c++17 -Wall -Wextra
```

Use:

```makefile
$(CXX)
$(CXXFLAGS)
```

Example:

```makefile
app:
 $(CXX) $(CXXFLAGS) main.cpp -o app
```

### 5. Separate Object Files

Makefile:

```makefile
CXX = g++
CXXFLAGS = -std=c++17 -Wall -Wextra

app: main.o calculator.o
 $(CXX) main.o calculator.o -o app

main.o: main.cpp calculator.h
 $(CXX) $(CXXFLAGS) -c main.cpp -o main.o

calculator.o: calculator.cpp calculator.h
 $(CXX) $(CXXFLAGS) -c calculator.cpp -o calculator.o

clean:
 rm -f app main.o calculator.o
```

Build:

```bash
make
```

### 6. Why Object Files Help

Suppose:

```text
main.cpp
calculator.cpp
```

are compiled separately.

If only `calculator.cpp` changes, Make can rebuild:

```text
calculator.o
```

and then relink the application instead of recompiling every source file.

This is one of the major reasons build systems track dependencies.

### 7. `clean`

Define:

```makefile
clean:
 rm -f app main.o calculator.o
```

Run:

```bash
make clean
```

This removes generated files.

### 8. `all` Target

A common pattern:

```makefile
.PHONY: all clean

all: app

app: main.o calculator.o
 g++ main.o calculator.o -o app

main.o: main.cpp calculator.h
 g++ -std=c++17 -Wall -Wextra -c main.cpp -o main.o

calculator.o: calculator.cpp calculator.h
 g++ -std=c++17 -Wall -Wextra -c calculator.cpp -o calculator.o

clean:
 rm -f app main.o calculator.o
```

Run:

```bash
make
```

or:

```bash
make all
```

### 9. `.PHONY`

Targets such as:

```makefile
clean
```

are not actual files.

Use:

```makefile
.PHONY: all clean
```

to tell Make they are commands/targets rather than files.

### 10. Automatic Variables

Useful Make variables include:

```text
$@
    target name

$<
    first prerequisite

$^
    all prerequisites
```

Example:

```makefile
app: main.o calculator.o
 $(CXX) $(CXXFLAGS) $^ -o $@
```

This expands approximately to:

```bash
g++ -std=c++17 -Wall -Wextra main.o calculator.o -o app
```

### 11. Pattern Rule

Instead of writing every `.o` rule:

```makefile
%.o: %.cpp
 $(CXX) $(CXXFLAGS) -c $< -o $@
```

This tells Make how to create:

```text
file.o
```

from:

```text
file.cpp
```

### 12. Practical Makefile

```makefile
CXX = g++
CXXFLAGS = -std=c++17 -Wall -Wextra -g

TARGET = app

SOURCES = main.cpp calculator.cpp
OBJECTS = $(SOURCES:.cpp=.o)

.PHONY: all clean

all: $(TARGET)

$(TARGET): $(OBJECTS)
 $(CXX) $(OBJECTS) -o $(TARGET)

%.o: %.cpp
 $(CXX) $(CXXFLAGS) -c $< -o $@

clean:
 rm -f $(OBJECTS) $(TARGET)
```

Commands:

```bash
make
```

```bash
make clean
```

### 13. Parallel Build

Make can build independent targets in parallel:

```bash
make -j4
```

Here `4` requests up to four jobs concurrently.

### 14. Inspect Make Commands

```bash
make
```

prints the commands it executes unless configured otherwise.

You can also use:

```bash
make -n
```

to show what would be executed without actually executing the commands.

### 15. Key Points

- `Makefile` defines build dependencies and commands.
- `make` executes the required build steps.
- Targets describe generated outputs.
- Prerequisites describe dependencies.
- Commands normally begin with a tab.
- `.PHONY` is useful for non-file targets.
- Object-file builds support incremental compilation.
- `make -j4` can build independent targets in parallel.
