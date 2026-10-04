# Create Thread

> Learn different ways to create and start threads in C++.

### 1. Include `<thread>`

```cpp
#include <iostream>
#include <thread>

using namespace std;
```

A thread is created by constructing a `std::thread` object.

### 2. Create Thread Using a Function

```cpp
#include <iostream>
#include <thread>

using namespace std;

void task()
{
    cout << "Task is running\n";
}

int main()
{
    thread t(task);

    t.join();

    return 0;
}
```

Here:

```cpp
thread t(task);
```

creates a thread that executes:

```cpp
task();
```

### 3. Thread with Function Arguments

```cpp
#include <iostream>
#include <thread>

using namespace std;

void greet(string name)
{
    cout << "Hello, " << name << '\n';
}

int main()
{
    thread t(greet, "Alice");

    t.join();

    return 0;
}
```

Possible output:

```text
Hello, Alice
```

### 4. Multiple Arguments

```cpp
#include <iostream>
#include <thread>

using namespace std;

void add(int a, int b)
{
    cout << "Sum = " << a + b << '\n';
}

int main()
{
    thread t(add, 10, 20);

    t.join();

    return 0;
}
```

Output:

```text
Sum = 30
```

### 5. Create Thread Using Lambda

A lambda can be passed directly to `std::thread`.

```cpp
#include <iostream>
#include <thread>

using namespace std;

int main()
{
    thread t([]()
    {
        cout << "Lambda thread is running\n";
    });

    t.join();

    return 0;
}
```

### 6. Lambda with Parameters

```cpp
#include <iostream>
#include <thread>

using namespace std;

int main()
{
    thread t([](int a, int b)
    {
        cout << "Sum = " << a + b << '\n';
    }, 10, 20);

    t.join();

    return 0;
}
```

Output:

```text
Sum = 30
```

### 7. Create Thread Using Function Object

A class with `operator()` can be used as a callable object.

```cpp
#include <iostream>
#include <thread>

using namespace std;

class Worker
{
public:
    void operator()()
    {
        cout << "Function object thread\n";
    }
};

int main()
{
    Worker worker;

    thread t(worker);

    t.join();

    return 0;
}
```

### 8. Temporary Function Object

The object can also be created directly.

```cpp
#include <iostream>
#include <thread>

using namespace std;

class Worker
{
public:
    void operator()()
    {
        cout << "Worker running\n";
    }
};

int main()
{
    thread t((Worker{}));

    t.join();

    return 0;
}
```

### 9. Create Thread Using Member Function

```cpp
#include <iostream>
#include <thread>

using namespace std;

class Printer
{
public:
    void print()
    {
        cout << "Member function is running\n";
    }
};

int main()
{
    Printer printer;

    thread t(&Printer::print, &printer);

    t.join();

    return 0;
}
```

The first argument is the member-function pointer:

```cpp
&Printer::print
```

The second argument identifies the object:

```cpp
&printer
```

### 10. Member Function with Arguments

```cpp
#include <iostream>
#include <thread>

using namespace std;

class Calculator
{
public:
    void add(int a, int b)
    {
        cout << "Result = " << a + b << '\n';
    }
};

int main()
{
    Calculator calculator;

    thread t(
        &Calculator::add,
        &calculator,
        10,
        20
    );

    t.join();

    return 0;
}
```

Output:

```text
Result = 30
```

### 11. Pass by Value

Thread arguments are normally copied into the thread's internal storage.

```cpp
#include <iostream>
#include <thread>

using namespace std;

void change(int value)
{
    value = 100;

    cout << "Thread value: "
         << value << '\n';
}

int main()
{
    int number = 10;

    thread t(change, number);

    t.join();

    cout << "Main value: "
         << number << '\n';

    return 0;
}
```

Output:

```text
Thread value: 100
Main value: 10
```

The thread changed its copy.

### 12. Pass by Reference Using `std::ref`

Use `std::ref()` when the thread function must receive a reference.

```cpp
#include <iostream>
#include <thread>
#include <functional>

using namespace std;

void change(int& value)
{
    value = 100;
}

int main()
{
    int number = 10;

    thread t(change, ref(number));

    t.join();

    cout << number << '\n';

    return 0;
}
```

Output:

```text
100
```

Include:

```cpp
#include <functional>
```

for `std::ref`.

### 13. Multiple Threads with Different Functions

```cpp
#include <iostream>
#include <thread>

using namespace std;

void download()
{
    cout << "Downloading...\n";
}

void process()
{
    cout << "Processing...\n";
}

void save()
{
    cout << "Saving...\n";
}

int main()
{
    thread t1(download);
    thread t2(process);
    thread t3(save);

    t1.join();
    t2.join();
    t3.join();

    return 0;
}
```

The three tasks may execute in different orders.

### 14. Check Whether a Thread Is Joinable

```cpp
#include <iostream>
#include <thread>

using namespace std;

void task()
{
    cout << "Task\n";
}

int main()
{
    thread t(task);

    if (t.joinable())
    {
        cout << "Thread is joinable\n";
    }

    t.join();

    if (!t.joinable())
    {
        cout << "Thread is no longer joinable\n";
    }

    return 0;
}
```

Possible output:

```text
Thread is joinable
Task
Thread is no longer joinable
```

### 15. Important Rule

A `std::thread` object that represents an active thread must be handled before it is destroyed.

For example:

```cpp
thread t(task);

t.join();
```

or:

```cpp
thread t(task);

t.detach();
```

If a joinable `std::thread` reaches its destructor, the program calls:

```cpp
std::terminate()
```

### 16. Key Points

- `std::thread` can execute functions, lambdas, function objects, and member functions.
- Thread arguments can be passed by value.
- Use `std::ref()` for reference arguments.
- Use `join()` or `detach()` before destroying a joinable thread.
- `joinable()` checks whether the thread object currently represents a joinable thread.
- Member functions require an object instance.
