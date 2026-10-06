# Greedy Algorithm

A **greedy algorithm** makes the best-looking local choice at each step with the goal of producing a globally optimal solution.

The important idea is:

```text
Choose the best available option now
            ↓
Continue
            ↓
Repeat
```

A greedy strategy does **not** automatically guarantee an optimal solution for every problem.

## Example: Coin Change

Suppose the available coins are:

```text
25, 10, 5, 1
```

Target:

```text
63
```

Greedy selection:

```text
63 - 25 = 38
38 - 25 = 13
13 - 10 = 3
3 - 1 = 2
2 - 1 = 1
1 - 1 = 0
```

Coins:

```text
25 + 25 + 10 + 1 + 1 + 1
```

## Implementation

```cpp
#include <iostream>
#include <vector>
using namespace std;

int main() {
    vector<int> coins = {
        25, 10, 5, 1
    };

    int amount = 63;

    for (int coin : coins) {
        while (amount >= coin) {
            cout << coin << " ";

            amount -= coin;
        }
    }

    return 0;
}
```

### Output

```text
25 25 10 1 1 1
```

## Activity Selection

Given activities with start and finish times, a common greedy strategy is to select the activity that finishes earliest.

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

struct Activity {
    int start;
    int finish;
};

int main() {
    vector<Activity> activities = {
        {1, 2},
        {3, 4},
        {0, 6},
        {5, 7},
        {8, 9},
        {5, 9}
    };

    sort(
        activities.begin(),
        activities.end(),
        [](const Activity& a, const Activity& b) {
            return a.finish < b.finish;
        }
    );

    int lastFinish = -1;

    for (const auto& activity : activities) {
        if (activity.start >= lastFinish) {
            cout << "("
                 << activity.start
                 << ", "
                 << activity.finish
                 << ") ";

            lastFinish = activity.finish;
        }
    }

    return 0;
}
```

### Output

```text
(1, 2) (3, 4) (5, 7) (8, 9)
```

## Greedy Choice

A greedy algorithm generally has:

1. A set of available choices.
2. A rule for selecting one choice.
3. A way to determine whether the choice is feasible.
4. A process that continues until the problem is complete.

## When Greedy Works

Greedy methods can produce optimal solutions when the problem has appropriate properties, commonly described as:

- **Greedy-choice property**
- **Optimal substructure**

## Greedy vs Dynamic Programming

|Feature|Greedy|Dynamic Programming|
|---|---|---|
|Decision|Local best choice|Considers subproblem relationships|
|Revisits choices|Usually no|Can reuse computed results|
|Implementation|Often simpler|Often more involved|
|Always optimal|No|Depends on correct DP formulation|
|Typical memory|Often lower|Often higher|

## Common Greedy Algorithms

- Activity selection
- Fractional knapsack
- Huffman coding
- Kruskal's algorithm
- Prim's algorithm
- Dijkstra's algorithm under its applicable assumptions

## Important Limitation

Greedy does not always produce an optimal solution.

For example, with coins:

```text
1, 3, 4
```

and target:

```text
6
```

Greedy chooses:

```text
4 + 1 + 1
```

using 3 coins.

But the optimal solution is:

```text
3 + 3
```

using 2 coins.

So the greedy strategy must be justified for the specific problem.
