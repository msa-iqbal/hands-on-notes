# new and delete

C++ provides `new` for dynamically allocating memory and `delete` for releasing dynamically allocated memory.

## Basic Example

```cpp
#include <iostream>

using namespace std;

int main() {
    int* number = new int;

    *number = 100;

    cout << "Number: " << *number << endl;

    delete number;

    return 0;
}
```

## Expected Output

```text
Number: 100
```

## Initialize During Allocation

```cpp
int* number = new int(100);

cout << *number << endl;

delete number;
```

## Multiple Variables

```cpp
int* a = new int(10);
int* b = new int(20);

cout << *a << " " << *b << endl;

delete a;
delete b;
```

## Important Rule

Every successful `new` allocation should eventually have a matching `delete`.

```cpp
int* value = new int(50);

delete value;
```

After deleting:

```cpp
value = nullptr;
```

Complete example:

```cpp
#include <iostream>

using namespace std;

int main() {
    int* number = new int(100);

    cout << *number << endl;

    delete number;
    number = nullptr;

    return 0;
}
```

## `new` and `delete`

| Keyword    | Purpose                                  |
| ---------- | ---------------------------------------- |
| `new`      | Dynamically allocates memory             |
| `delete`   | Releases memory allocated for one object |
| `new[]`    | Dynamically allocates an array           |
| `delete[]` | Releases a dynamically allocated array   |
