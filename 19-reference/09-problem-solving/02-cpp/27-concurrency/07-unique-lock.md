# Unique Lock

> Learn how `std::unique_lock` provides flexible mutex ownership.

### 1. What Is `std::unique_lock`?

`std::unique_lock` is an RAII-based mutex ownership wrapper with more flexibility than `std::lock_guard`.

Include:

```cpp
#include <mutex>
```

Basic usage:

```cpp
unique_lock<mutex> lock(mtx);
```

The mutex is locked when the object is constructed and unlocked when the object leaves scope.

### 2. Basic Example

```cpp
#include <iostream>
#include <thread>
#include <mutex>

using namespace std;

mutex mtx;

void task()
{
    unique_lock<mutex> lock(mtx);

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

### 3. Unlock Manually

Unlike `lock_guard`, `unique_lock` can be manually unlocked.

```cpp
#include <iostream>
#include <thread>
#include <mutex>

using namespace std;

mutex mtx;

void task()
{
    unique_lock<mutex> lock(mtx);

    cout << "Protected section\n";

    lock.unlock();

    cout << "Outside protected section\n";
}

int main()
{
    thread t(task);

    t.join();

    return 0;
}
```

### 4. Lock Again

A `unique_lock` can be locked again after being unlocked.

```cpp
#include <iostream>
#include <mutex>

using namespace std;

int main()
{
    mutex mtx;

    unique_lock<mutex> lock(mtx);

    cout << "First lock\n";

    lock.unlock();

    cout << "Unlocked\n";

    lock.lock();

    cout << "Locked again\n";

    lock.unlock();

    return 0;
}
```

Output:

```text
First lock
Unlocked
Locked again
```

### 5. Deferred Locking

Use:

```cpp
defer_lock
```

to construct the `unique_lock` without immediately locking the mutex.

```cpp
#include <iostream>
#include <mutex>

using namespace std;

int main()
{
    mutex mtx;

    unique_lock<mutex> lock(
        mtx,
        defer_lock
    );

    cout << "Lock has not been acquired yet\n";

    lock.lock();

    cout << "Lock acquired\n";

    lock.unlock();

    return 0;
}
```

### 6. `try_lock()`

```cpp
#include <iostream>
#include <mutex>

using namespace std;

int main()
{
    mutex mtx;

    unique_lock<mutex> lock(
        mtx,
        defer_lock
    );

    if (lock.try_lock())
    {
        cout << "Lock acquired\n";
    }
    else
    {
        cout << "Lock unavailable\n";
    }

    return 0;
}
```

### 7. Release Ownership

`release()` gives up ownership of the mutex without unlocking it.

```cpp
mutex mtx;

unique_lock<mutex> lock(mtx);

mutex* ptr = lock.release();
```

After:

```cpp
lock.release();
```

the `unique_lock` no longer owns the mutex.

The mutex must then be managed appropriately.

Because `release()` transfers responsibility, it should be used carefully.

### 8. Move a `unique_lock`

`unique_lock` is movable.

```cpp
#include <iostream>
#include <mutex>

using namespace std;

int main()
{
    mutex mtx;

    unique_lock<mutex> lock1(mtx);

    unique_lock<mutex> lock2 = move(lock1);

    cout << boolalpha
         << lock1.owns_lock()
         << '\n';

    cout << lock2.owns_lock()
         << '\n';

    return 0;
}
```

Typical output:

```text
false
true
```

### 9. `owns_lock()`

Check whether the `unique_lock` currently owns a mutex.

```cpp
unique_lock<mutex> lock(mtx);

if (lock.owns_lock())
{
    cout << "Lock is owned\n";
}
```

### 10. Release Lock Before Expensive Work

```cpp
#include <iostream>
#include <thread>
#include <mutex>
#include <chrono>

using namespace std;

mutex mtx;
int value = 0;

void task()
{
    unique_lock<mutex> lock(mtx);

    value++;

    lock.unlock();

    this_thread::sleep_for(
        chrono::seconds(1)
    );

    cout << "Expensive work completed\n";
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

The mutex is not held during the one-second sleep.

### 11. `unique_lock` vs `lock_guard`

|Feature|`lock_guard`|`unique_lock`|
|---|---|---|
|RAII|Yes|Yes|
|Automatically locks|Yes|Yes|
|Manual unlock|No|Yes|
|Lock again|No|Yes|
|Deferred locking|No|Yes|
|`try_lock()`|No|Yes|
|Movable|No|Yes|
|Flexible ownership|Limited|High|

### 12. Condition Variables

`std::condition_variable` requires a lock type such as `std::unique_lock`.

Example:

```cpp
unique_lock<mutex> lock(mtx);

cv.wait(lock);
```

The condition-variable pattern is covered in the next file.

### 13. Key Points

- `unique_lock` provides RAII mutex ownership.
- It is more flexible than `lock_guard`.
- It can be manually unlocked and locked again.
- It supports deferred locking.
- It supports `try_lock()`.
- It can transfer ownership with move semantics.
- `condition_variable` commonly uses `unique_lock`.
- Use it when simple `lock_guard` behavior is insufficient.
