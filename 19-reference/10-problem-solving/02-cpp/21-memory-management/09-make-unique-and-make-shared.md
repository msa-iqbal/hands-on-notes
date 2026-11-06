# make_unique and make_shared

C++ provides:

```cpp
std::make_unique
```

and:

```cpp
std::make_shared
```

for creating objects managed by smart pointers.

Include:

```cpp
#include <memory>
```

## make_unique

```cpp
#include <iostream>
#include <memory>

using namespace std;

int main() {
    auto number =
        make_unique<int>(100);

    cout << *number << endl;

    return 0;
}
```

## Expected Output

```text
100
```

Equivalent conceptually to:

```cpp
unique_ptr<int> number(
    new int(100)
);
```

But `make_unique` is the preferred modern C++ approach.

## make_shared

```cpp
#include <iostream>
#include <memory>

using namespace std;

int main() {
    auto number =
        make_shared<int>(100);

    cout << *number << endl;

    return 0;
}
```

## Expected Output

```text
100
```

## Creating Objects

```cpp
#include <iostream>
#include <memory>

using namespace std;

class Student {
public:
    string name;
    int age;

    Student(string n, int a)
        : name(n), age(a) {
    }

    void display() const {
        cout << name << " "
             << age << endl;
    }
};

int main() {
    auto student =
        make_unique<Student>("Muhammad", 25);

    student->display();

    return 0;
}
```

## Expected Output

```text
Muhammad 25
```

## Comparison

|Function|Smart Pointer|Ownership|
|---|---|---|
|`make_unique<T>()`|`unique_ptr<T>`|Exclusive|
|`make_shared<T>()`|`shared_ptr<T>`|Shared|

## Prefer make Functions

Prefer:

```cpp
auto object = make_unique<MyClass>();
```

over:

```cpp
unique_ptr<MyClass> object(
    new MyClass()
);
```

And:

```cpp
auto object = make_shared<MyClass>();
```

over:

```cpp
shared_ptr<MyClass> object(
    new MyClass()
);
```

The `make_*` functions provide clearer ownership intent and integrate naturally with RAII.
