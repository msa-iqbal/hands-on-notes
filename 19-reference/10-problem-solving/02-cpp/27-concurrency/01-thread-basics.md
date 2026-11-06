# Thread Basics

> Learn the fundamentals of threads and concurrent execution in C++.

#### 1. What Is a Thread?

A **thread** is an independent path of execution inside a program.

A C++ program normally starts with one thread called the **main thread**.

Additional threads can be created to perform work concurrently.

```text
Program
│
├── Main Thread
│
├── Worker Thread 1
│
├── Worker Thread 2
│
└── Worker Thread 3
```

C++ provides `std::thread` through the `<thread>` header.

#### 2. Include `<thread>`

```cpp
#include <iostream>
#include <thread>

using namespace std;

int main()
{
    cout << "Main thread is running\n";

    return 0;
}
```

Compile:

```bash
g++ -std=c++17 main.cpp -o main
```

#### 3. Basic Thread

```cpp
#include <iostream>
#include <thread>

using namespace std;

void worker()
{
    cout << "Worker thread is running\n";
}

int main()
{
    thread t(worker);

    t.join();

    cout << "Main thread is running\n";

    return 0;
}
```

Possible output:

```text
Worker thread is running
Main thread is running
```

The exact execution order can vary because thread scheduling is controlled by the operating system.

#### 4. Main Thread and Worker Thread

```cpp
#include <iostream>
#include <thread>

using namespace std;

void task()
{
    for (int i = 1; i <= 5; i++)
    {
        cout << "Worker: " << i << '\n';
    }
}

int main()
{
    cout << "Main started\n";

    thread t(task);

    t.join();

    cout << "Main finished\n";

    return 0;
}
```

Possible output:

```text
Main started
Worker: 1
Worker: 2
Worker: 3
Worker: 4
Worker: 5
Main finished
```

### 5. Get Current Thread ID

Use:

```cpp
this_thread::get_id()
```

Example:

```cpp
#include <iostream>
#include <thread>

using namespace std;

void worker()
{
    cout << "Worker ID: "
         << this_thread::get_id()
         << '\n';
}

int main()
{
    cout << "Main ID: "
         << this_thread::get_id()
         << '\n';

    thread t(worker);

    t.join();

    return 0;
}
```

Output will contain implementation-dependent thread IDs.

### 6. Sleep a Thread

Use:

```cpp
this_thread::sleep_for()
```

Example:

```cpp
#include <iostream>
#include <thread>
#include <chrono>

using namespace std;

void worker()
{
    cout << "Worker started\n";

    this_thread::sleep_for(chrono::seconds(2));

    cout << "Worker finished\n";
}

int main()
{
    thread t(worker);

    t.join();

    return 0;
}
```

The worker pauses for approximately two seconds.

### 7. Sleep for Milliseconds

```cpp
#include <iostream>
#include <thread>
#include <chrono>

using namespace std;

int main()
{
    for (int i = 1; i <= 5; i++)
    {
        cout << i << '\n';

        this_thread::sleep_for(
            chrono::milliseconds(500)
        );
    }

    return 0;
}
```

### 8. Yield the Current Thread

`yield()` gives the scheduler an opportunity to run another ready thread.

```cpp
#include <iostream>
#include <thread>

using namespace std;

void worker()
{
    for (int i = 1; i <= 5; i++)
    {
        cout << "Worker: " << i << '\n';

        this_thread::yield();
    }
}

int main()
{
    thread t(worker);

    t.join();

    return 0;
}
```

`yield()` does not guarantee that another thread will run immediately.

### 9. Multiple Threads

```cpp
#include <iostream>
#include <thread>

using namespace std;

void task(int id)
{
    cout << "Thread " << id << " is running\n";
}

int main()
{
    thread t1(task, 1);
    thread t2(task, 2);
    thread t3(task, 3);

    t1.join();
    t2.join();
    t3.join();

    return 0;
}
```

Possible output:

```text
Thread 1 is running
Thread 3 is running
Thread 2 is running
```

The order is not guaranteed.

### 10. Concurrency vs Parallelism

#### Concurrency

Multiple tasks make progress during overlapping periods.

```text
Task A: ███   ███
Task B:   ███   ███
```

#### Parallelism

Multiple tasks execute simultaneously on multiple CPU cores.

```text
CPU 1: Task A █████████
CPU 2: Task B █████████
```

A multithreaded C++ program may use concurrency, parallelism, or both depending on the hardware and scheduler.

### 11. Thread Scheduling

The operating system decides when threads execute.

Therefore, this:

```cpp
thread t1(task1);
thread t2(task2);
```

does not mean:

```text
t1 always executes first
t2 always executes second
```

The output may change between executions.

### 12. Key Points

- `std::thread` represents a thread of execution.
- Include `<thread>` to use it.
- `std::this_thread` provides operations for the current thread.
- `get_id()` gets the current thread ID.
- `sleep_for()` pauses the current thread.
- `yield()` gives the scheduler an opportunity to run another thread.
- Thread execution order is generally nondeterministic.
- A thread must be properly managed before its `std::thread` object is destroyed.
- `join()` and `detach()` are covered separately.
