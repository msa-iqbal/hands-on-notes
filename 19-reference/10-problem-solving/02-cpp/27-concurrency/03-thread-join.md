# Thread Join

> Learn how `std::thread::join()` waits for a thread to finish.

### 1. What Is `join()`?

`join()` makes the calling thread wait until the target thread finishes.

```cpp
thread t(task);

t.join();
```

The main thread waits for `task()` to complete.

### 2. Basic Example

```cpp
#include <iostream>
#include <thread>

using namespace std;

void task()
{
    cout << "Worker started\n";
    cout << "Worker finished\n";
}

int main()
{
    thread t(task);

    t.join();

    cout << "Main finished\n";

    return 0;
}
```

Possible output:

```text
Worker started
Worker finished
Main finished
```

### 3. Join with Sleep

```cpp
#include <iostream>
#include <thread>
#include <chrono>

using namespace std;

void task()
{
    cout << "Worker started\n";

    this_thread::sleep_for(
        chrono::seconds(2)
    );

    cout << "Worker finished\n";
}

int main()
{
    thread t(task);

    t.join();

    cout << "Main finished\n";

    return 0;
}
```

The main thread waits approximately two seconds for the worker.

### 4. Why `join()` Is Important

Consider:

```cpp
#include <iostream>
#include <thread>
#include <chrono>

using namespace std;

void task()
{
    this_thread::sleep_for(
        chrono::seconds(3)
    );

    cout << "Task finished\n";
}

int main()
{
    thread t(task);

    t.join();

    cout << "Program finished\n";

    return 0;
}
```

The program does not finish its `main()` function until the worker has completed.

### 5. Join Multiple Threads

```cpp
#include <iostream>
#include <thread>

using namespace std;

void task(int id)
{
    cout << "Thread " << id << '\n';
}

int main()
{
    thread t1(task, 1);
    thread t2(task, 2);
    thread t3(task, 3);

    t1.join();
    t2.join();
    t3.join();

    cout << "All threads finished\n";

    return 0;
}
```

Possible output:

```text
Thread 1
Thread 3
Thread 2
All threads finished
```

The order of the worker output is not guaranteed.

### 6. Join Order

Suppose:

```cpp
thread t1(task1);
thread t2(task2);

t1.join();
t2.join();
```

The main thread waits for `t1` first.

However, `t2` may already have completed while the main thread was waiting for `t1`.

```text
Main
 │
 ├── wait for t1 ──────────────┐
 │                             │
 │                     t1 finishes
 │
 ├── join t2
 │
 └── continue
```

Joining order does not necessarily determine execution order.

### 7. Check `joinable()`

Before calling `join()`, you can check:

```cpp
if (t.joinable())
{
    t.join();
}
```

Example:

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
        t.join();
    }

    return 0;
}
```

### 8. Calling `join()` Twice

This is incorrect:

```cpp
thread t(task);

t.join();
t.join();
```

After the first `join()`:

```cpp
t.joinable()
```

is false.

Use:

```cpp
if (t.joinable())
{
    t.join();
}
```

when code structure requires a conditional join.

### 9. Move a Thread and Join It

A thread object can transfer ownership using `std::move`.

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
    thread t1(task);

    thread t2 = move(t1);

    t2.join();

    return 0;
}
```

After the move:

```cpp
t1.joinable() == false
```

and:

```cpp
t2.joinable() == true
```

### 10. Safe Join Pattern

A common pattern is:

```cpp
thread t(task);

if (t.joinable())
{
    t.join();
}
```

This prevents calling `join()` on a thread that is no longer joinable.

### 11. Join Several Threads

```cpp
#include <iostream>
#include <thread>
#include <vector>

using namespace std;

void task(int id)
{
    cout << "Task " << id << " completed\n";
}

int main()
{
    vector<thread> threads;

    for (int i = 1; i <= 5; i++)
    {
        threads.emplace_back(task, i);
    }

    for (auto& t : threads)
    {
        if (t.joinable())
        {
            t.join();
        }
    }

    cout << "All tasks completed\n";

    return 0;
}
```

This is useful when the number of threads is determined dynamically.

### 12. Thread Lifetime

A thread object and the actual thread of execution are related but distinct concepts.

```text
std::thread object
       │
       └── represents → OS-managed execution
```

Calling:

```cpp
t.join();
```

waits for that execution to finish and makes the `std::thread` object non-joinable.

### 13. `join()` vs `detach()`

|Feature|`join()`|`detach()`|
||||
|Main waits|Yes|No|
|Thread continues independently|No|Yes|
|Can join later|No|No|
|Thread object becomes non-joinable|Yes|Yes|
|Ownership remains structured|Yes|No|

Use `join()` when the program needs to wait for the work to finish.

`detach()` is covered in the next file.

### 14. Common Mistake

Incorrect:

```cpp
int main()
{
    thread t(task);

    return 0;
}
```

If `t` is still joinable when its destructor runs, the program calls:

```cpp
std::terminate()
```

Correct:

```cpp
int main()
{
    thread t(task);

    t.join();

    return 0;
}
```

### 15. Complete Example

```cpp
#include <iostream>
#include <thread>
#include <chrono>

using namespace std;

void download()
{
    for (int i = 1; i <= 5; i++)
    {
        cout << "Downloading: " << i << '\n';

        this_thread::sleep_for(
            chrono::milliseconds(300)
        );
    }

    cout << "Download completed\n";
}

int main()
{
    cout << "Starting download...\n";

    thread worker(download);

    if (worker.joinable())
    {
        worker.join();
    }

    cout << "Main program continues\n";

    return 0;
}
```

Possible output:

```text
Starting download...
Downloading: 1
Downloading: 2
Downloading: 3
Downloading: 4
Downloading: 5
Download completed
Main program continues
```

### 16. Key Points

- `join()` waits for a thread to finish.
- The calling thread is blocked while waiting.
- After `join()`, the thread object is no longer joinable.
- Do not call `join()` twice on the same thread object.
- Use `joinable()` when a conditional join is appropriate.
- Every joinable `std::thread` must be joined or detached before destruction.
- Joining does not determine the worker threads' execution order.
- `join()` is usually the straightforward way to maintain explicit thread ownership.
