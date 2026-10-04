# RAII

RAII means:

> **Resource Acquisition Is Initialization**

It is a fundamental C++ resource-management technique.

The basic idea is that a resource is acquired during object initialization and released automatically when the object is destroyed.

Resources can include:

- Memory
- Files
- Locks
- Sockets
- Database connections

## Basic Example

```cpp
#include <iostream>

using namespace std;

class Resource {
public:
    Resource() {
        cout << "Resource acquired." << endl;
    }

    ~Resource() {
        cout << "Resource released." << endl;
    }
};

int main() {
    {
        Resource resource;

        cout << "Using resource." << endl;
    }

    cout << "Back in main." << endl;

    return 0;
}
```

## Expected Output

```text
Resource acquired.
Using resource.
Resource released.
Back in main.
```

The destructor runs automatically when `resource` goes out of scope.

## RAII with File Streams

```cpp
#include <fstream>

int main() {
    {
        std::ofstream file("data.txt");

        file << "Hello, C++!";
    }

    return 0;
}
```

When `file` goes out of scope, its destructor releases the associated file resource.

## RAII with Memory

Instead of:

```cpp
int* number = new int(100);

delete number;
```

modern C++ generally prefers an owning smart pointer:

```cpp
auto number = std::make_unique<int>(100);
```

The memory is automatically released when `number` goes out of scope.

## Main Benefit

RAII reduces manual resource-management code and helps ensure resources are released correctly, including when exceptions cause control flow to leave a scope.
