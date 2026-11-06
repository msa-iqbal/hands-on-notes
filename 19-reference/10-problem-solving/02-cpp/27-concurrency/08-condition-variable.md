# Condition Variable

> Learn how threads can wait efficiently for a condition using `std::condition_variable`.

### 1. What Is a Condition Variable?

A condition variable allows one thread to wait until another thread signals that some condition or state may have changed.

C++ provides:

```cpp
std::condition_variable
```

through:

```cpp
#include <condition_variable>
```

It is commonly used with:

```cpp
std::mutex
std::unique_lock
```

### 2. Basic Structure

```cpp
mutex mtx;
condition_variable cv;
bool ready = false;
```

Waiting thread:

```cpp
unique_lock<mutex> lock(mtx);

cv.wait(lock, []()
{
    return ready;
});
```

Signaling thread:

```cpp
{
    lock_guard<mutex> lock(mtx);

    ready = true;
}

cv.notify_one();
```

### 3. Why Not Just Use a Loop?

A thread could repeatedly check:

```cpp
while (!ready)
{
    // keep checking
}
```

This is called **busy waiting** and can waste CPU resources.

A condition variable allows the waiting thread to sleep until notified.

### 4. Basic Wait Example

```cpp
#include <iostream>
#include <thread>
#include <mutex>
#include <condition_variable>
#include <chrono>

using namespace std;

mutex mtx;
condition_variable cv;
bool ready = false;

void worker()
{
    unique_lock<mutex> lock(mtx);

    cv.wait(lock, []()
    {
        return ready;
    });

    cout << "Worker received signal\n";
}

int main()
{
    thread t(worker);

    this_thread::sleep_for(
        chrono::seconds(1)
    );

    {
        lock_guard<mutex> lock(mtx);

        ready = true;
    }

    cv.notify_one();

    t.join();

    return 0;
}
```

Output:

```text
Worker received signal
```

### 5. `wait(lock, predicate)`

Preferred form:

```cpp
cv.wait(lock, predicate);
```

Example:

```cpp
cv.wait(lock, []()
{
    return ready;
});
```

The thread waits until the predicate returns `true`.

### 6. Spurious Wakeups

A condition-variable wait can return even when the desired condition is not actually true.

This is called a **spurious wakeup**.

Therefore, prefer:

```cpp
cv.wait(lock, []()
{
    return ready;
});
```

rather than relying on:

```cpp
cv.wait(lock);
```

without checking the condition.

Equivalent manual pattern:

```cpp
while (!ready)
{
    cv.wait(lock);
}
```

### 7. `notify_one()`

Use:

```cpp
cv.notify_one();
```

to wake one waiting thread.

Example:

```cpp
ready = true;

cv.notify_one();
```

### 8. `notify_all()`

Use:

```cpp
cv.notify_all();
```

to wake all waiting threads.

```cpp
ready = true;

cv.notify_all();
```

Each awakened thread still needs to check its condition.

### 9. Producer-Consumer Example

A common condition-variable pattern is a producer and consumer.

```cpp
#include <iostream>
#include <thread>
#include <mutex>
#include <condition_variable>
#include <queue>

using namespace std;

queue<int> data;
mutex mtx;
condition_variable cv;

void producer()
{
    for (int i = 1; i <= 5; i++)
    {
        {
            lock_guard<mutex> lock(mtx);

            data.push(i);

            cout << "Produced: "
                 << i
                 << '\n';
        }

        cv.notify_one();
    }
}

void consumer()
{
    for (int i = 1; i <= 5; i++)
    {
        unique_lock<mutex> lock(mtx);

        cv.wait(lock, []()
        {
            return !data.empty();
        });

        int value = data.front();

        data.pop();

        lock.unlock();

        cout << "Consumed: "
             << value
             << '\n';
    }
}

int main()
{
    thread p(producer);
    thread c(consumer);

    p.join();
    c.join();

    return 0;
}
```

Possible output:

```text
Produced: 1
Produced: 2
Consumed: 1
Produced: 3
Consumed: 2
Produced: 4
Consumed: 3
Produced: 5
Consumed: 4
Consumed: 5
```

The exact ordering can vary.

### 10. Why Use `unique_lock`?

The waiting operation:

```cpp
cv.wait(lock, predicate);
```

needs a `unique_lock`.

While waiting:

```text
unique_lock owns mutex
       ↓
wait()
       ↓
mutex temporarily released
       ↓
thread sleeps
       ↓
notification
       ↓
mutex reacquired
       ↓
predicate checked
```

This allows another thread to acquire the mutex and change the shared state.

### 11. Complete Producer-Consumer Example

```cpp
#include <iostream>
#include <thread>
#include <mutex>
#include <condition_variable>
#include <queue>
#include <chrono>

using namespace std;

queue<int> tasks;

mutex mtx;
condition_variable cv;

bool finished = false;

void producer()
{
    for (int i = 1; i <= 10; i++)
    {
        {
            lock_guard<mutex> lock(mtx);

            tasks.push(i);
        }

        cv.notify_one();

        this_thread::sleep_for(
            chrono::milliseconds(100)
        );
    }

    {
        lock_guard<mutex> lock(mtx);

        finished = true;
    }

    cv.notify_one();
}

void consumer()
{
    while (true)
    {
        unique_lock<mutex> lock(mtx);

        cv.wait(lock, []()
        {
            return !tasks.empty() || finished;
        });

        if (tasks.empty() && finished)
        {
            break;
        }

        int task = tasks.front();

        tasks.pop();

        lock.unlock();

        cout << "Processing task "
             << task
             << '\n';
    }
}

int main()
{
    thread p(producer);
    thread c(consumer);

    p.join();
    c.join();

    cout << "All tasks completed\n";

    return 0;
}
```

Possible output:

```text
Processing task 1
Processing task 2
Processing task 3
Processing task 4
Processing task 5
Processing task 6
Processing task 7
Processing task 8
Processing task 9
Processing task 10
All tasks completed
```

### 12. Condition Variable State

A condition variable itself does not represent the condition.

The shared state does.

For example:

```cpp
bool ready = false;
```

or:

```cpp
!tasks.empty()
```

The mutex protects that shared state.

The condition variable only provides an efficient waiting and notification mechanism.

### 13. Key Points

- `condition_variable` allows threads to wait efficiently.
- It is normally used with a mutex.
- `wait()` requires a `unique_lock`.
- Prefer `wait(lock, predicate)`.
- Predicates protect against spurious wakeups.
- `notify_one()` wakes one waiting thread.
- `notify_all()` wakes all waiting threads.
- The shared condition/state must be protected by the mutex.
- Producer-consumer queues are a common use case.
