# Range Library

The C++20 **Ranges library** provides tools for working with collections and sequences more declaratively.

Common features include:

- `std::ranges`
- `std::views`
- `filter`
- `transform`
- `sort`
- range pipelines

Compile with C++20:

```bash
g++ -std=c++20 program.cpp -o program
```

## Example 1: ranges::sort()

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

int main() {
    vector<int> numbers = {5, 2, 8, 1, 3};

    ranges::sort(numbers);

    for (int number : numbers) {
        cout << number << " ";
    }

    return 0;
}
```

### Output

```text
1 2 3 5 8
```

## Example 2: views::filter()

`filter` creates a view containing elements that satisfy a condition.

```cpp
#include <iostream>
#include <vector>
#include <ranges>
using namespace std;

int main() {
    vector<int> numbers = {
        1, 2, 3, 4, 5, 6
    };

    auto even = numbers
        | views::filter([](int n) {
            return n % 2 == 0;
        });

    for (int number : even) {
        cout << number << " ";
    }

    return 0;
}
```

### Output

```text
2 4 6
```

## Example 3: views::transform()

```cpp
#include <iostream>
#include <vector>
#include <ranges>
using namespace std;

int main() {
    vector<int> numbers = {1, 2, 3, 4, 5};

    auto squares = numbers
        | views::transform([](int n) {
            return n * n;
        });

    for (int number : squares) {
        cout << number << " ";
    }

    return 0;
}
```

### Output

```text
1 4 9 16 25
```

## Example 4: Filter and Transform

Ranges can be combined into pipelines.

```cpp
#include <iostream>
#include <vector>
#include <ranges>
using namespace std;

int main() {
    vector<int> numbers = {
        1, 2, 3, 4, 5, 6
    };

    auto result = numbers
        | views::filter([](int n) {
            return n % 2 == 0;
        })
        | views::transform([](int n) {
            return n * 10;
        });

    for (int number : result) {
        cout << number << " ";
    }

    return 0;
}
```

### Output

```text
20 40 60
```

## Example 5: Take First Elements

```cpp
#include <iostream>
#include <vector>
#include <ranges>
using namespace std;

int main() {
    vector<int> numbers = {
        10, 20, 30, 40, 50
    };

    auto firstThree = numbers
        | views::take(3);

    for (int number : firstThree) {
        cout << number << " ";
    }

    return 0;
}
```

### Output

```text
10 20 30
```

## Example 6: Drop Elements

```cpp
#include <iostream>
#include <vector>
#include <ranges>
using namespace std;

int main() {
    vector<int> numbers = {
        10, 20, 30, 40, 50
    };

    auto result = numbers
        | views::drop(2);

    for (int number : result) {
        cout << number << " ";
    }

    return 0;
}
```

### Output

```text
30 40 50
```

## Example 7: String Filtering

```cpp
#include <iostream>
#include <vector>
#include <string>
#include <ranges>
using namespace std;

int main() {
    vector<string> names = {
        "Alice",
        "Bob",
        "Andrew",
        "Charlie"
    };

    auto result = names
        | views::filter([](const string& name) {
            return name[0] == 'A';
        });

    for (const auto& name : result) {
        cout << name << endl;
    }

    return 0;
}
```

### Output

```text
Alice
Andrew
```

## Example 8: Pipeline

A range pipeline can express multiple operations in sequence.

```cpp
#include <iostream>
#include <vector>
#include <ranges>
using namespace std;

int main() {
    vector<int> numbers = {
        1, 2, 3, 4, 5, 6, 7, 8
    };

    auto result = numbers
        | views::filter([](int n) {
            return n % 2 == 0;
        })
        | views::transform([](int n) {
            return n * n;
        })
        | views::take(3);

    for (int number : result) {
        cout << number << " ";
    }

    return 0;
}
```

### Output

```text
4 16 36
```

## Views Are Lazy

A view generally does not immediately create a new container containing all results.

For example:

```cpp
auto result = numbers
    | views::filter(predicate);
```

The view describes how to access matching elements.

## Common C++20 Range Components

|Component|Purpose|
|---|---|
|`ranges::sort`|Sort a range|
|`views::filter`|Select elements|
|`views::transform`|Transform elements|
|`views::take`|Take first N elements|
|`views::drop`|Skip first N elements|
|`views::reverse`|Reverse view|
|`views::iota`|Generate values|

## Compile

```bash
g++ -std=c++20 program.cpp -o program
```

Run:

```bash
./program
```

## Key Points

- C++20 introduced the modern Ranges library.
- `std::ranges` provides range-aware algorithms.
- `std::views` provides lazy range adaptors.
- Range pipelines use the `|` operator.
- Views can be combined to create readable data-processing pipelines.
- Use `-std=c++20` when compiling these examples.
