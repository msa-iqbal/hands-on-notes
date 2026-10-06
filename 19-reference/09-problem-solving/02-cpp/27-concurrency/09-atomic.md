# Atomic

> Learn how `std::atomic` provides atomic operations for shared variables.

### 1. What Is `std::atomic`?

`std::atomic<T>` provides operations on a shared object that are atomic with respect to other atomic operations on that object.

Include:

```cpp
#include <atomic>
```

Example:

```cpp
atomic<int> counter = 0;
```

### 2. Atomic Counter

```cpp
#include <iostream>
#include <thread>
#include <atomic>

using namespace std;

atomic<int> counter = 0;

void increment()
{
    for (int i = 0; i < 100000; i++)
    {
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

The increment operation on the atomic counter is synchronized.

### 3. `load()`

Read the current value:

```cpp
atomic<int> value = 100;

int result = value.load();
```

Example:

```cpp
#include <iostream>
#include <atomic>

using namespace std;

int main()
{
    atomic<int> value = 100;

    cout << value.load() << '\n';

    return 0;
}
```

Output:

```text
100
```

### 4. `store()`

Change the value:

```cpp
atomic<int> value = 100;

value.store(200);
```

Example:

```cpp
#include <iostream>
#include <atomic>

using namespace std;

int main()
{
    atomic<int> value = 100;

    value.store(200);

    cout << value.load() << '\n';

    return 0;
}
```

Output:

```text
200
```

### 5. `fetch_add()`

Atomically add a value:

```cpp
atomic<int> counter = 10;

counter.fetch_add(5);
```

Now:

```text
counter = 15
```

Example:

```cpp
#include <iostream>
#include <atomic>

using namespace std;

int main()
{
    atomic<int> counter = 10;

    int oldValue = counter.fetch_add(5);

    cout << "Old = "
         << oldValue
         << '\n';

    cout << "New = "
         << counter.load()
         << '\n';

    return 0;
}
```

Output:

```text
Old = 10
New = 15
```

### 6. `fetch_sub()`

```cpp
#include <iostream>
#include <atomic>

using namespace std;

int main()
{
    atomic<int> counter = 10;

    int oldValue = counter.fetch_sub(3);

    cout << "Old = "
         << oldValue
         << '\n';

    cout << "New = "
         << counter.load()
         << '\n';

    return 0;
}
```

Output:

```text
Old = 10
New = 7
```

### 7. `exchange()`

`exchange()` replaces the value and returns the old value.

```cpp
#include <iostream>
#include <atomic>

using namespace std;

int main()
{
    atomic<int> value = 100;

    int oldValue = value.exchange(200);

    cout << "Old = "
         << oldValue
         << '\n';

    cout << "New = "
         << value.load()
         << '\n';

    return 0;
}
```

Output:

```text
Old = 100
New = 200
```

### 8. Atomic Boolean

An atomic boolean is useful for simple shared state.

```cpp
#include <iostream>
#include <thread>
#include <atomic>
#include <chrono>

using namespace std;

atomic<bool> running = true;

void worker()
{
    while (running.load())
    {
        cout << "Working...\n";

        this_thread::sleep_for(
            chrono::milliseconds(200)
        );
    }

    cout << "Worker stopped\n";
}

int main()
{
    thread t(worker);

    this_thread::sleep_for(
        chrono::seconds(1)
    );

    running.store(false);

    t.join();

    return 0;
}
```

Possible output:

```text
Working...
Working...
Working...
Working...
Working...
Worker stopped
```

### 9. Compare and Exchange

`compare_exchange` performs a conditional update.

Conceptually:

```text
if atomic value == expected
    replace it with desired
else
    update expected with actual value
```

Example:

```cpp
#include <iostream>
#include <atomic>

using namespace std;

int main()
{
    atomic<int> value = 10;

    int expected = 10;

    bool changed = value.compare_exchange_strong(
        expected,
        20
    );

    cout << boolalpha
         << "Changed: "
         << changed
         << '\n';

    cout << "Value: "
         << value.load()
         << '\n';

    return 0;
}
```

Output:

```text
Changed: true
Value: 20
```

### 10. Failed Compare Exchange

```cpp
#include <iostream>
#include <atomic>

using namespace std;

int main()
{
    atomic<int> value = 10;

    int expected = 5;

    bool changed = value.compare_exchange_strong(
        expected,
        20
    );

    cout << boolalpha
         << "Changed: "
         << changed
         << '\n';

    cout << "Expected: "
         << expected
         << '\n';

    cout << "Value: "
         << value.load()
         << '\n';

    return 0;
}
```

Output:

```text
Changed: false
Expected: 10
Value: 10
```

Because the actual value was `10`, not `5`.

### 11. Atomic Counter with Multiple Threads

```cpp
#include <iostream>
#include <thread>
#include <atomic>
#include <vector>

using namespace std;

atomic<int> counter = 0;

void work()
{
    for (int i = 0; i < 50000; i++)
    {
        counter.fetch_add(1);
    }
}

int main()
{
    vector<thread> threads;

    for (int i = 0; i < 4; i++)
    {
        threads.emplace_back(work);
    }

    for (auto& t : threads)
    {
        t.join();
    }

    cout << "Counter = "
         << counter.load()
         << '\n';

    return 0;
}
```

Output:

```text
Counter = 200000
```

### 12. Atomic vs Mutex

For a simple shared counter:

```cpp
atomic<int> counter = 0;
```

may be enough.

A mutex is more appropriate when an operation involves multiple pieces of state that must change together.

Example:

```cpp
int balance;
int transaction_count;
```

If both must maintain an invariant, protecting the combined operation with a mutex can be more appropriate than using separate atomics.

### 13. Atomic Does Not Mean Everything Is Automatically Safe

This:

```cpp
atomic<int> a;
atomic<int> b;
```

does not automatically make a multi-variable operation atomic as one transaction.

For example:

```cpp
a++;
b++;
```

contains two separate atomic operations.

If another thread must observe a consistent relationship between `a` and `b`, a mutex or another synchronization design may be necessary.

### 14. Atomic Operations

Common operations include:

```cpp
load()
store()
exchange()
fetch_add()
fetch_sub()
compare_exchange_strong()
compare_exchange_weak()
```

Operators such as:

```cpp
counter++;
```

can also perform atomic operations when `counter` is an appropriate `std::atomic` type.

### 15. Complete Example

```cpp
#include <iostream>
#include <thread>
#include <atomic>
#include <vector>

using namespace std;

atomic<int> completed = 0;

void process(int id)
{
    cout << "Task "
         << id
         << " started\n";

    for (int i = 0; i < 100000; i++)
    {
        completed.fetch_add(1);
    }

    cout << "Task "
         << id
         << " finished\n";
}

int main()
{
    vector<thread> threads;

    for (int i = 1; i <= 4; i++)
    {
        threads.emplace_back(process, i);
    }

    for (auto& t : threads)
    {
        t.join();
    }

    cout << "Completed operations = "
         << completed.load()
         << '\n';

    return 0;
}
```

Possible output:

```text
Task 1 started
Task 2 started
Task 3 started
Task 4 started
Task 2 finished
Task 1 finished
Task 4 finished
Task 3 finished
Completed operations = 400000
```

The ordering of the messages can vary.

### 16. Key Points

- `std::atomic` provides atomic operations on supported types.
- Use `<atomic>`.
- `load()` reads the value.
- `store()` writes the value.
- `fetch_add()` and `fetch_sub()` perform atomic arithmetic.
- `exchange()` replaces a value atomically.
- `compare_exchange_*()` supports conditional updates.
- Atomics are useful for simple shared state such as counters and flags.
- Atomic variables do not automatically make a multi-variable invariant atomic.
- A mutex is often more appropriate for complex critical sections.
