# Standard Headers

C++ provides a large standard library organized into headers.

Headers provide declarations for standard functionality.

## Example 1: iostream

Used for console input and output.

```cpp
#include <iostream>

int main() {
    std::cout << "Hello" << std::endl;

    return 0;
}
```

## Example 2: string

```cpp
#include <iostream>
#include <string>

int main() {
    std::string name = "Alice";

    std::cout << name << std::endl;

    return 0;
}
```

## Example 3: vector

```cpp
#include <iostream>
#include <vector>

int main() {
    std::vector<int> numbers = {
        10, 20, 30
    };

    for (int number : numbers) {
        std::cout << number << " ";
    }

    return 0;
}
```

## Example 4: algorithm

```cpp
#include <iostream>
#include <algorithm>
#include <vector>

int main() {
    std::vector<int> numbers = {
        5, 2, 8, 1
    };

    std::sort(numbers.begin(), numbers.end());

    for (int number : numbers) {
        std::cout << number << " ";
    }

    return 0;
}
```

### Output

```text
1 2 5 8
```

## Example 5: map

```cpp
#include <iostream>
#include <map>
#include <string>

int main() {
    std::map<std::string, int> ages;

    ages["Alice"] = 25;
    ages["Bob"] = 30;

    for (const auto& [name, age] : ages) {
        std::cout << name << ": "
                  << age << std::endl;
    }

    return 0;
}
```

## Common Standard Headers

|Header|Common Purpose|
|---|---|
|`<iostream>`|Input/output|
|`<string>`|`std::string`|
|`<vector>`|Dynamic array|
|`<array>`|Fixed-size array|
|`<list>`|Linked list|
|`<map>`|Ordered key-value container|
|`<unordered_map>`|Hash map|
|`<set>`|Ordered unique values|
|`<algorithm>`|Algorithms|
|`<numeric>`|Numeric algorithms|
|`<memory>`|Smart pointers|
|`<utility>`|Utility types/functions|
|`<tuple>`|Tuples|
|`<optional>`|`std::optional`|
|`<variant>`|`std::variant`|
|`<any>`|`std::any`|
|`<fstream>`|File handling|
|`<sstream>`|String streams|
|`<iomanip>`|Output formatting|
|`<cmath>`|Mathematical functions|
|`<chrono>`|Time utilities|
|`<thread>`|Threads|
|`<mutex>`|Synchronization|

## Angle Brackets vs Quotes

Standard library headers normally use:

```cpp
#include <iostream>
```

Project headers normally use:

```cpp
#include "my-header.h"
```

## Key Points

- Headers provide declarations and library interfaces.
- Include only the headers needed by your source file.
- Standard headers use angle brackets.
- Project headers commonly use double quotes.
