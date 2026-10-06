# Lock Guard

> Learn how `std::lock_guard` provides automatic mutex locking and unlocking.

### 1. What Is `std::lock_guard`?

`std::lock_guard` is an RAII wrapper around a mutex.

When a `lock_guard` is created:

```cpp
lock_guard<mutex> lock(mtx);
```

the mutex is locked.

When the `lock_guard` leaves scope:

```text
scope ends
   ↓
lock_guard destructor
   ↓
mutex automatically unlocked
```

Include:

```cpp
#include <mutex>
```

### 2. Basic Example

```cpp
#include <iostream>
#include <thread>
#include <mutex>

using namespace std;

mutex mtx;

void task()
{
    lock_guard<mutex> lock(mtx);

    cout << "Protected section\n";
}

int main()
{
    thread t1(task);
    thread t2(task);

    t1.join();
    t2.join();

    return 0;
}
```

Only one thread executes the protected section at a time.

### 3. Protect a Counter

```cpp
#include <iostream>
#include <thread>
#include <mutex>

using namespace std;

int counter = 0;
mutex mtx;

void increment()
{
    for (int i = 0; i < 100000; i++)
    {
        lock_guard<mutex> lock(mtx);

        counter++;
    }
}

int main()
{
    thread t1(increment);
    thread t2(increment);

    t1.join();
    t2.join();

    cout << "Counter = "
         << counter
         << '\n';

    return 0;
}
```

Output:

```text
Counter = 200000
```

### 4. Automatic Unlocking

```cpp
void task()
{
    {
        lock_guard<mutex> lock(mtx);

        cout << "Critical section\n";
    }

    cout << "Mutex has been released\n";
}
```

The mutex is released automatically when the inner block ends.

### 5. Why RAII Is Safer

Manual approach:

```cpp
mtx.lock();

do_work();

mtx.unlock();
```

RAII approach:

```cpp
lock_guard<mutex> lock(mtx);

do_work();
```

If `do_work()` throws an exception, the destructor of `lock_guard` still releases the mutex during stack unwinding.

### 6. Shared Data Example

```cpp
#include <iostream>
#include <thread>
#include <mutex>

using namespace std;

class Counter
{
private:
    int value = 0;
    mutex mtx;

public:
    void increment()
    {
        lock_guard<mutex> lock(mtx);

        value++;
    }

    int get()
    {
        lock_guard<mutex> lock(mtx);

        return value;
    }
};

void work(Counter& counter)
{
    for (int i = 0; i < 10000; i++)
    {
        counter.increment();
    }
}

int main()
{
    Counter counter;

    thread t1(work, ref(counter));
    thread t2(work, ref(counter));

    t1.join();
    t2.join();

    cout << counter.get() << '\n';

    return 0;
}
```

Output:

```text
20000
```

### 7. Multiple Critical Sections

```cpp
#include <iostream>
#include <thread>
#include <mutex>

using namespace std;

mutex mtx;

void task()
{
    {
        lock_guard<mutex> lock(mtx);

        cout << "First critical section\n";
    }

    cout << "Outside critical section\n";

    {
        lock_guard<mutex> lock(mtx);

        cout << "Second critical section\n";
    }
}

int main()
{
    thread t(task);

    t.join();

    return 0;
}
```

The first lock is released before the second lock is acquired.

### 8. Limit Lock Duration

Avoid holding a mutex while performing unrelated expensive work.

Instead of:

```cpp
lock_guard<mutex> lock(mtx);

update_data();

expensive_operation();
```

prefer:

```cpp
{
    lock_guard<mutex> lock(mtx);

    update_data();
}

expensive_operation();
```

This reduces the time other threads must wait for the mutex.

### 9. `lock_guard` with `std::mutex`

```cpp
mutex mtx;

void task()
{
    lock_guard<mutex> lock(mtx);

    // protected code
}
```

The variable:

```cpp
lock
```

owns the lock for the lifetime of the guard.

### 10. Different Mutex Names

You can choose any valid variable name:

```cpp
lock_guard<mutex> guard(mtx);
```

or:

```cpp
lock_guard<mutex> lock(mtx);
```

or:

```cpp
lock_guard<mutex> mutex_lock(mtx);
```

The important part is the lifetime of the object.

### 11. Lock Guard and Scope

```cpp
void task()
{
    cout << "Before lock\n";

    {
        lock_guard<mutex> lock(mtx);

        cout << "Inside lock\n";
    }

    cout << "After lock\n";
}
```

Execution:

```text
Before lock
    ↓
lock mutex
    ↓
Inside lock
    ↓
unlock mutex
    ↓
After lock
```

### 12. Lock Guard vs Manual Locking

|Feature|Manual `lock()`|`lock_guard`|
|---|---|---|
|Lock manually|Yes|No|
|Unlock manually|Yes|No|
|RAII|No|Yes|
|Exception safety|Easier to get wrong|Better|
|Simple scoped locking|More verbose|Yes|
|Recommended for basic scoped locks|Less preferred|Yes|

### 13. Important Limitation

`lock_guard` is intentionally simple.

Once constructed, it owns the mutex until it goes out of scope.

You cannot use it to:

```cpp
unlock();
```

and later:

```cpp
lock();
```

within the same object's lifetime.

For more flexible locking, use `std::unique_lock`.

### 14. Complete Example

```cpp
#include <iostream>
#include <thread>
#include <mutex>
#include <vector>

using namespace std;

int counter = 0;
mutex counter_mutex;

void increment()
{
    for (int i = 0; i < 50000; i++)
    {
        lock_guard<mutex> lock(counter_mutex);

        ++counter;
    }
}

int main()
{
    vector<thread> threads;

    for (int i = 0; i < 4; i++)
    {
        threads.emplace_back(increment);
    }

    for (auto& t : threads)
    {
        t.join();
    }

    cout << "Final counter = "
         << counter
         << '\n';

    return 0;
}
```

Output:

```text
Final counter = 200000
```

### 15. Key Points

- `std::lock_guard` uses RAII for mutex management.
- Construction locks the mutex.
- Destruction unlocks the mutex.
- It is excellent for simple scoped critical sections.
- It is safer than manual `lock()`/`unlock()`.
- Use braces to control how long the mutex remains locked.
- `lock_guard` cannot be manually unlocked and relocked.
- Use `std::unique_lock` when more flexible lock management is required.
