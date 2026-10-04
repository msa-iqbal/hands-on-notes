# Backtracking

Backtracking explores possible solutions by:

1. Making a choice.
2. Exploring that choice.
3. Undoing the choice when it does not lead to a valid solution.
4. Trying another choice.

```text
Choose
  ↓
Explore
  ↓
Valid?
 /   \
Yes   No
 |     |
Continue
       ↓
     Undo
       ↓
   Try another
```

Backtracking is commonly used for:

- Permutations
- Combinations
- Subsets
- N-Queens
- Sudoku
- Maze problems

## 1. Generate Subsets

```cpp
#include <iostream>
#include <vector>
using namespace std;

void generateSubsets(
    const vector<int>& numbers,
    int index,
    vector<int>& current
) {
    if (index == numbers.size()) {
        cout << "{ ";

        for (int number : current) {
            cout << number << " ";
        }

        cout << "}" << endl;

        return;
    }

    // Do not choose the current element.
    generateSubsets(
        numbers,
        index + 1,
        current
    );

    // Choose the current element.
    current.push_back(numbers[index]);

    generateSubsets(
        numbers,
        index + 1,
        current
    );

    // Undo the choice.
    current.pop_back();
}

int main() {
    vector<int> numbers = {
        1, 2, 3
    };

    vector<int> current;

    generateSubsets(
        numbers,
        0,
        current
    );

    return 0;
}
```

### Output

```text
{ }
{ 3 }
{ 2 }
{ 2 3 }
{ 1 }
{ 1 3 }
{ 1 2 }
{ 1 2 3 }
```

## 2. Generate Permutations

```cpp
#include <iostream>
#include <vector>
using namespace std;

void permutations(
    vector<int>& numbers,
    int index
) {
    if (index == numbers.size()) {
        for (int number : numbers) {
            cout << number << " ";
        }

        cout << endl;
        return;
    }

    for (int i = index; i < numbers.size(); i++) {
        swap(numbers[index], numbers[i]);

        permutations(
            numbers,
            index + 1
        );

        swap(numbers[index], numbers[i]);
    }
}

int main() {
    vector<int> numbers = {
        1, 2, 3
    };

    permutations(numbers, 0);

    return 0;
}
```

### Output

```text
1 2 3
1 3 2
2 1 3
2 3 1
3 2 1
3 1 2
```

The ordering of generated permutations depends on the implementation.

## 3. Combination Sum

Find combinations whose values add up to a target.

```cpp
#include <iostream>
#include <vector>
using namespace std;

void combinationSum(
    const vector<int>& numbers,
    int index,
    int target,
    vector<int>& current
) {
    if (target == 0) {
        cout << "{ ";

        for (int number : current) {
            cout << number << " ";
        }

        cout << "}" << endl;
        return;
    }

    if (target < 0 || index == numbers.size()) {
        return;
    }

    // Choose current number.
    current.push_back(numbers[index]);

    combinationSum(
        numbers,
        index,
        target - numbers[index],
        current
    );

    // Undo choice.
    current.pop_back();

    // Skip current number.
    combinationSum(
        numbers,
        index + 1,
        target,
        current
    );
}

int main() {
    vector<int> numbers = {
        2, 3, 6, 7
    };

    vector<int> current;

    combinationSum(
        numbers,
        0,
        7,
        current
    );

    return 0;
}
```

### Output

```text
{ 2 2 3 }
{ 7 }
```

## 4. Simple Maze Backtracking

```cpp
#include <iostream>
#include <vector>
using namespace std;

bool solveMaze(
    const vector<vector<int>>& maze,
    vector<vector<int>>& path,
    int row,
    int col
) {
    int n = maze.size();

    if (
        row < 0 ||
        col < 0 ||
        row >= n ||
        col >= n ||
        maze[row][col] == 0 ||
        path[row][col] == 1
    ) {
        return false;
    }

    path[row][col] = 1;

    if (row == n - 1 && col == n - 1) {
        return true;
    }

    if (solveMaze(
        maze,
        path,
        row + 1,
        col
    )) {
        return true;
    }

    if (solveMaze(
        maze,
        path,
        row,
        col + 1
    )) {
        return true;
    }

    if (solveMaze(
        maze,
        path,
        row - 1,
        col
    )) {
        return true;
    }

    if (solveMaze(
        maze,
        path,
        row,
        col - 1
    )) {
        return true;
    }

    // Backtrack.
    path[row][col] = 0;

    return false;
}

int main() {
    vector<vector<int>> maze = {
        {1, 0, 0, 0},
        {1, 1, 0, 1},
        {0, 1, 0, 0},
        {1, 1, 1, 1}
    };

    int n = maze.size();

    vector<vector<int>> path(
        n,
        vector<int>(n, 0)
    );

    if (solveMaze(
        maze,
        path,
        0,
        0
    )) {
        for (const auto& row : path) {
            for (int cell : row) {
                cout << cell << " ";
            }

            cout << endl;
        }
    } else {
        cout << "No path found" << endl;
    }

    return 0;
}
```

### Output

```text
1 0 0 0
1 1 0 0
0 1 0 0
0 1 1 1
```

## Backtracking Pattern

The general pattern is:

```cpp
void backtrack(...) {
    if (solutionFound) {
        return;
    }

    for (each possible choice) {
        if (choiceIsValid) {
            makeChoice();

            backtrack(...);

            undoChoice();
        }
    }
}
```

## Backtracking vs Brute Force

Backtracking is related to brute-force search, but it can **prune** branches that cannot produce valid solutions.

Example:

```text
All possibilities
       |
       +---- Invalid branch → stop
       |
       +---- Valid branch → continue
       |
       +---- Invalid branch → stop
       |
       +---- Valid branch → continue
```

## Key Points

- Build a solution incrementally.
- Check whether a choice is valid.
- Undo choices when necessary.
- Prune impossible branches.
- Commonly implemented with recursion.
- Worst-case complexity can still be exponential for many problems.
