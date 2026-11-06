# Mutex

> Learn how `std::mutex` protects shared data from concurrent access.

### 1. What Is a Mutex?

A **mutex** (mutual exclusion) allows only one thread at a time to enter a protected critical section.

C++ provides:

```cpp
std::mutex
```

through:

```cpp
#include <mutex>
```

### 2. Why Do We Need a Mutex?

Consider a shared counter:

```cpp
int counter = 0;
```

Two threads simultaneously execute:

```cpp
counter++;
```

The operation is not necessarily a single indivisible machine operation.

It can conceptually involve:

```text
read
 ↓
modify
 ↓
write
```

Two threads can interfere with each other.

This can cause a **data race**, which results in undefined behavior.

### 3. Basic Mutex

```cpp
#include <iostream>
#include <thread>
#include <mutex>

using namespace std;

mutex mtx;

void task()
{
    mtx.lock();

    cout << "Thread entered critical section\n";

    mtx.unlock();
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

Only one thread can hold `mtx` at a time.

### 4. Protect a Shared Counter

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
        mtx.lock();

        counter++;

        mtx.unlock();
    }
}

int main()
{
    thread t1(increment);
    thread t2(increment);

    t1.join();
    t2.join();

    cout << "Counter = " << counter << '\n';

    return 0;
}
```

Output:

```text
Counter = 200000
```

The mutex protects the increment operation.

### 5. Critical Section

A **critical section** is code that accesses shared state and must not be executed concurrently by conflicting threads.

```cpp
mtx.lock();

counter++;

mtx.unlock();
```

The protected section is:

```cpp
counter++;
```

### 6. Manual `lock()` and `unlock()`

```cpp
mtx.lock();

cout << "Protected data\n";

mtx.unlock();
```

This works, but manual locking is error-prone.

For example:

```cpp
mtx.lock();

some_function();

mtx.unlock();
```

If `some_function()` throws an exception, `unlock()` might never execute.

That can leave the mutex locked.

RAII wrappers such as `std::lock_guard` are safer.

### 7. `try_lock()`

`try_lock()` attempts to acquire the mutex without blocking indefinitely.

```cpp
#include <iostream>
#include <thread>
#include <mutex>

using namespace std;

mutex mtx;

void task()
{
    if (mtx.try_lock())
    {
        cout << "Mutex acquired\n";

        mtx.unlock();
    }
    else
    {
        cout << "Mutex is busy\n";
    }
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

One possible result is:

```text
Mutex acquired
Mutex is busy
```

The exact output depends on scheduling.

### 8. Protect Shared Data

```cpp
#include <iostream>
#include <thread>
#include <mutex>

using namespace std;

int balance = 1000;
mutex mtx;

void deposit(int amount)
{
    mtx.lock();

    balance += amount;

    mtx.unlock();
}

int main()
{
    thread t1(deposit, 500);
    thread t2(deposit, 300);

    t1.join();
    t2.join();

    cout << "Balance = " << balance << '\n';

    return 0;
}
```

Output:

```text
Balance = 1800
```

### 9. Mutex with Multiple Shared Operations

```cpp
#include <iostream>
#include <thread>
#include <mutex>

using namespace std;

int value = 0;
mutex mtx;

void task()
{
    for (int i = 0; i < 5; i++)
    {
        mtx.lock();

        value++;

        cout << "Value = "
             << value
             << '\n';

        mtx.unlock();
    }
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

The output order can vary, but access to `value` is synchronized.

### 10. Mutex Does Not Automatically Protect Everything

Suppose:

```cpp
int a;
int b;
mutex mtx;
```

If one operation needs both values to maintain an invariant, the entire related operation should be protected consistently.

A mutex only protects the code that actually uses it.

### 11. Mutex and Deadlock

Two mutexes can produce a deadlock if threads acquire them in conflicting orders.

```text
Thread 1:
lock A
lock B

Thread 2:
lock B
lock A
```

Possible situation:

```text
Thread 1 owns A → waits for B

Thread 2 owns B → waits for A
```

Both wait forever.

Lock ordering and higher-level locking facilities can prevent this class of problem.

### 12. Key Points

- `std::mutex` provides mutual exclusion.
- Only one thread can own a mutex at a time.
- Use a mutex to protect shared mutable state.
- `lock()` blocks until the mutex is acquired.
- `unlock()` releases it.
- `try_lock()` attempts acquisition without waiting for the lock.
- Manual locking can be error-prone.
- RAII locking with `std::lock_guard` is safer.
- Inconsistent locking can still cause data races.
- Multiple mutexes require careful deadlock prevention.
