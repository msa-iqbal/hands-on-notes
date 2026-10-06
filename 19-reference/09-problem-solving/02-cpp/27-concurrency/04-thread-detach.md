# Thread Detach

> Learn how `std::thread::detach()` allows a thread to execute independently.

### 1. What Is `detach()`?

`detach()` separates the thread of execution from its `std::thread` object.

```cpp
thread t(task);

t.detach();
```

After detaching:

```text
std::thread object
       X
       │
       │ detached
       ▼
Independent thread
```

The thread continues independently.

### 2. Basic Example

```cpp
#include <iostream>
#include <thread>
#include <chrono>

using namespace std;

void task()
{
    cout << "Detached thread started\n";

    this_thread::sleep_for(
        chrono::milliseconds(500)
    );

    cout << "Detached thread finished\n";
}

int main()
{
    thread t(task);

    t.detach();

    this_thread::sleep_for(
        chrono::seconds(1)
    );

    cout << "Main finished\n";

    return 0;
}
```

Possible output:

```text
Detached thread started
Detached thread finished
Main finished
```

The sleep in `main()` gives the detached thread time to finish.

### 3. `joinable()` After `detach()`

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

    cout << boolalpha
         << t.joinable()
         << '\n';

    t.detach();

    cout << t.joinable()
         << '\n';

    return 0;
}
```

Typical output:

```text
true
false
```

### 4. Detached Thread Cannot Be Joined

This is incorrect:

```cpp
thread t(task);

t.detach();

t.join();
```

After `detach()`:

```cpp
t.joinable()
```

is false.

Therefore, there is no longer a thread represented by `t` that can be joined through that object.

### 5. Join vs Detach

###### Join

```cpp
thread t(task);

t.join();
```

Main waits.

###### Detach

```cpp
thread t(task);

t.detach();
```

Main does not wait for that thread through `t`.

### 6. Important Lifetime Problem

Be careful when a detached thread accesses local variables.

Bad design:

```cpp
#include <iostream>
#include <thread>

using namespace std;

void task(int& value)
{
    cout << value << '\n';
}

void start()
{
    int value = 100;

    thread t(task, ref(value));

    t.detach();
}
```

The function may return before the detached thread accesses `value`.

Then:

```text
value lifetime ends
       ↓
detached thread accesses invalid reference
       ↓
undefined behavior
```

This is one of the major dangers of detached threads.

### 7. Safer Detached Thread

Pass data by value when appropriate.

```cpp
#include <iostream>
#include <thread>
#include <chrono>

using namespace std;

void task(int value)
{
    this_thread::sleep_for(
        chrono::milliseconds(100)
    );

    cout << "Value = " << value << '\n';
}

int main()
{
    thread t(task, 100);

    t.detach();

    this_thread::sleep_for(
        chrono::milliseconds(200)
    );

    return 0;
}
```

The integer argument is copied into the thread's invocation.

### 8. Detached Thread and Program Lifetime

A detached thread does not keep the process alive after `main()` finishes.

```cpp
#include <iostream>
#include <thread>
#include <chrono>

using namespace std;

void task()
{
    this_thread::sleep_for(
        chrono::seconds(5)
    );

    cout << "Finished\n";
}

int main()
{
    thread t(task);

    t.detach();

    cout << "Main finished\n";

    return 0;
}
```

The process may terminate before the detached thread prints:

```text
Finished
```

Therefore, do not use `detach()` as a way to avoid waiting without considering process and object lifetimes.

### 9. Conditional Detach

```cpp
#include <iostream>
#include <thread>

using namespace std;

void task()
{
    cout << "Task running\n";
}

int main()
{
    thread t(task);

    if (t.joinable())
    {
        t.detach();
    }

    return 0;
}
```

### 10. When Detach May Be Useful

A detached thread can be appropriate when:

- The work is intentionally independent.
- The thread does not depend on short-lived local objects.
- The application has a clear lifetime strategy.
- No caller needs to wait for completion.
- Shared state is safely managed.

For many applications, explicit ownership and joining are easier to reason about.

### 11. Key Points

- `detach()` separates a thread from its `std::thread` object.
- The detached thread continues independently.
- A detached thread cannot be joined later through that object.
- `joinable()` becomes false after `detach()`.
- Detached threads can create difficult lifetime problems.
- The process can end before a detached thread completes.
- Be especially careful with references, pointers, and local variables.
- Use detachment deliberately rather than simply to avoid `join()`.
