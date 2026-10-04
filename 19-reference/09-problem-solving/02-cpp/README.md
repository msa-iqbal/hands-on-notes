# C++ Programming & Problem Solving

> C++ programming problems organized by topic and learning sequence.

A practical collection of C++ programming examples, problem-solving exercises, language concepts, STL usage, algorithms, data structures, and small projects.

The goal is to learn C++ progressively — from basic syntax and programming problems to object-oriented programming, modern C++, STL, concurrency, and project development.

---

## Directory Tree

```text
02-cpp/
│
├── README.md
│
├── 01-basics/
│   ├── 01.01-hello-world.md
│   ├── 01.02-comments.md
│   ├── 01.03-variables.md
│   ├── 01.04-constants.md
│   ├── 01.05-data-types.md
│   ├── 01.06-input-output.md
│   ├── 01.07-type-casting.md
│   ├── 01.08-operators.md
│   ├── 01.09-operator-precedence.md
│   └── 01.10-enumeration.md
│
├── 02-character/
│   ├── 02.01-find-ascii-value.md
│   ├── 02.02-lowercase-to-uppercase.md
│   ├── 02.03-uppercase-to-lowercase.md
│   ├── 02.04-lowercase-to-uppercase-using-function.md
│   ├── 02.05-uppercase-to-lowercase-using-function.md
│   ├── 02.06-vowel-or-consonant.md
│   ├── 02.07-capital-or-small-letter.md
│   └── 02.08-character-classification.md
│
├── 03-number-conversion/
│   ├── 03.01-decimal-to-binary.md
│   ├── 03.02-binary-to-decimal.md
│   ├── 03.03-decimal-to-octal.md
│   ├── 03.04-octal-to-decimal.md
│   ├── 03.05-decimal-to-hexadecimal.md
│   ├── 03.06-hexadecimal-to-decimal.md
│   ├── 03.07-binary-to-octal.md
│   ├── 03.08-octal-to-binary.md
│   ├── 03.09-binary-to-hexadecimal.md
│   ├── 03.10-hexadecimal-to-binary.md
│   ├── 03.11-octal-to-hexadecimal.md
│   └── 03.12-hexadecimal-to-octal.md
│
├── 04-basic-calculation/
│   ├── 04.01-summation-and-average.md
│   ├── 04.02-area-of-triangle.md
│   ├── 04.03-area-of-triangle-from-three-values.md
│   ├── 04.04-area-of-rectangle.md
│   ├── 04.05-area-of-circle.md
│   ├── 04.06-celsius-to-fahrenheit.md
│   ├── 04.07-fahrenheit-to-celsius.md
│   ├── 04.08-swap-with-temporary-variable.md
│   ├── 04.09-swap-without-temporary-variable.md
│   └── 04.10-quadratic-equation.md
│
├── 05-conditions/
│   ├── 05.01-even-or-odd.md
│   ├── 05.02-positive-or-negative.md
│   ├── 05.03-student-grade-system.md
│   ├── 05.04-largest-of-three-numbers.md
│   ├── 05.05-smallest-of-three-numbers.md
│   ├── 05.06-leap-year.md
│   ├── 05.07-calculator-using-switch.md
│   ├── 05.08-nested-if.md
│   └── 05.09-ternary-operator.md
│
├── 06-loops-and-numbers/
│   ├── 06.01-multiplication-table.md
│   ├── 06.02-factorial.md
│   ├── 06.03-prime-number.md
│   ├── 06.04-prime-numbers-in-range.md
│   ├── 06.05-gcd-and-lcm.md
│   ├── 06.06-sum-of-digits.md
│   ├── 06.07-reverse-number.md
│   ├── 06.08-palindrome-number.md
│   ├── 06.09-armstrong-number.md
│   ├── 06.10-armstrong-numbers-in-range.md
│   ├── 06.11-count-digit-in-integer.md
│   ├── 06.12-strong-number.md
│   ├── 06.13-fibonacci-series.md
│   ├── 06.14-natural-number-series-sum.md
│   ├── 06.15-even-number-series-sum.md
│   ├── 06.16-odd-number-series-sum.md
│   ├── 06.17-square-series-sum.md
│   ├── 06.18-cube-series-sum.md
│   ├── 06.19-harmonic-series-sum.md
│   └── 06.20-alternating-series-sum.md
│
├── 07-patterns/
│   ├── 07.01-number-triangles/
│   │   ├── 07.01.01-right-angle-triangle-01.md
│   │   ├── 07.01.02-right-angle-triangle-02.md
│   │   ├── 07.01.03-right-angle-triangle-03.md
│   │   ├── 07.01.04-right-angle-triangle-04.md
│   │   ├── 07.01.05-right-angle-triangle-05.md
│   │   ├── 07.01.06-right-angle-triangle-06.md
│   │   ├── 07.01.07-left-angle-triangle-01.md
│   │   ├── 07.01.08-left-angle-triangle-02.md
│   │   ├── 07.01.09-left-angle-triangle-03.md
│   │   ├── 07.01.10-left-angle-triangle-04.md
│   │   ├── 07.01.11-left-angle-triangle-05.md
│   │   └── 07.01.12-left-angle-triangle-06.md
│   │
│   ├── 07.02-binary-triangles/
│   │   ├── 07.02.01-right-angle-triangle-01.md
│   │   ├── 07.02.02-right-angle-triangle-02.md
│   │   ├── 07.02.03-right-angle-triangle-03.md
│   │   ├── 07.02.04-right-angle-triangle-04.md
│   │   ├── 07.02.05-right-angle-triangle-05.md
│   │   ├── 07.02.06-right-angle-triangle-06.md
│   │   ├── 07.02.07-left-angle-triangle-01.md
│   │   ├── 07.02.08-left-angle-triangle-02.md
│   │   ├── 07.02.09-left-angle-triangle-03.md
│   │   ├── 07.02.10-left-angle-triangle-04.md
│   │   ├── 07.02.11-left-angle-triangle-05.md
│   │   └── 07.02.12-left-angle-triangle-06.md
│   │
│   ├── 07.03-alphabetic-triangles/
│   │   ├── 07.03.01-right-angle-triangle-01.md
│   │   ├── 07.03.02-right-angle-triangle-02.md
│   │   ├── 07.03.03-right-angle-triangle-03.md
│   │   ├── 07.03.04-right-angle-triangle-04.md
│   │   ├── 07.03.05-right-angle-triangle-05.md
│   │   ├── 07.03.06-right-angle-triangle-06.md
│   │   ├── 07.03.07-left-angle-triangle-01.md
│   │   ├── 07.03.08-left-angle-triangle-02.md
│   │   ├── 07.03.09-left-angle-triangle-03.md
│   │   ├── 07.03.10-left-angle-triangle-04.md
│   │   ├── 07.03.11-left-angle-triangle-05.md
│   │   └── 07.03.12-left-angle-triangle-06.md
│   │
│   ├── 07.04-symbol-triangles/
│   │   ├── 07.04.01-right-angle-triangle-01.md
│   │   ├── 07.04.02-right-angle-triangle-02.md
│   │   ├── 07.04.03-right-angle-triangle-03.md
│   │   ├── 07.04.04-right-angle-triangle-04.md
│   │   ├── 07.04.05-right-angle-triangle-05.md
│   │   ├── 07.04.06-right-angle-triangle-06.md
│   │   ├── 07.04.07-left-angle-triangle-01.md
│   │   ├── 07.04.08-left-angle-triangle-02.md
│   │   └── 07.04.09-left-angle-triangle-03.md
│   │
│   ├── 07.05-flow-patterns/
│   │   ├── 07.05.01-pattern-01.md
│   │   ├── 07.05.02-pattern-02.md
│   │   ├── 07.05.03-pattern-03.md
│   │   ├── 07.05.04-pattern-04.md
│   │   └── 07.05.05-pattern-05.md
│   │
│   ├── 07.06-pyramid-patterns/
│   │   ├── 07.06.01-pyramid-01.md
│   │   ├── 07.06.02-pyramid-02.md
│   │   ├── 07.06.03-pyramid-03.md
│   │   ├── 07.06.04-pyramid-04.md
│   │   ├── 07.06.05-pyramid-05.md
│   │   ├── 07.06.06-pyramid-06.md
│   │   ├── 07.06.07-pattern-07.md
│   │   ├── 07.06.08-pattern-08.md
│   │   ├── 07.06.09-pattern-09.md
│   │   ├── 07.06.10-pyramid-10.md
│   │   ├── 07.06.11-pattern-11.md
│   │   ├── 07.06.12-pattern-12.md
│   │   ├── 07.06.13-pyramid-13.md
│   │   ├── 07.06.14-pattern-14.md
│   │   ├── 07.06.15-pattern-15.md
│   │   └── 07.06.16-pyramid-16.md
│   │
│   ├── 07.07-rectangle-star.md
│   ├── 07.08-triangle-star.md
│   ├── 07.09-x-shape-star.md
│   └── 07.10-floyd-triangle.md
│
├── 08-arrays/
│   ├── 08.01-input-array.md
│   ├── 08.02-print-array.md
│   ├── 08.03-summation-and-average.md
│   ├── 08.04-maximum-and-minimum.md
│   ├── 08.05-copy-array.md
│   ├── 08.06-reverse-array.md
│   ├── 08.07-delete-array-element.md
│   ├── 08.08-insert-array-element.md
│   ├── 08.09-linear-search.md
│   ├── 08.10-binary-search.md
│   ├── 08.11-sort-array.md
│   └── 08.12-remove-duplicates.md
│
├── 09-matrices/
│   ├── 09.01-create-matrix.md
│   ├── 09.02-matrix-addition.md
│   ├── 09.03-matrix-subtraction.md
│   ├── 09.04-matrix-multiplication.md
│   ├── 09.05-transpose-matrix.md
│   ├── 09.06-diagonal-sum.md
│   ├── 09.07-upper-and-lower-triangle-sum.md
│   └── 09.08-symmetric-matrix.md
│
├── 10-strings/
│   ├── 10.01-string-input.md
│   ├── 10.02-string-length.md
│   ├── 10.03-string-concatenation.md
│   ├── 10.04-string-comparison.md
│   ├── 10.05-reverse-string.md
│   ├── 10.06-string-palindrome.md
│   ├── 10.07-string-uppercase-lowercase.md
│   ├── 10.08-count-vowels-consonants-digits-words.md
│   ├── 10.09-count-capital-small-letters-digits.md
│   ├── 10.10-substring-search.md
│   └── 10.11-string-tokenization.md
│
├── 11-functions/
│   ├── 11.01-function-definition.md
│   ├── 11.02-function-parameters.md
│   ├── 11.03-return-value.md
│   ├── 11.04-default-arguments.md
│   ├── 11.05-function-overloading.md
│   ├── 11.06-inline-function.md
│   ├── 11.07-passing-array-to-function.md
│   ├── 11.08-passing-string-to-function.md
│   ├── 11.09-reference-parameters.md
│   └── 11.10-recursive-function.md
│
├── 12-pointers-and-references/
│   ├── 12.01-pointer-basics.md
│   ├── 12.02-pointer-to-variable.md
│   ├── 12.03-pointer-arithmetic.md
│   ├── 12.04-pointer-and-array.md
│   ├── 12.05-pointer-to-function.md
│   ├── 12.06-pointer-to-pointer.md
│   ├── 12.07-reference-variable.md
│   ├── 12.08-pointer-vs-reference.md
│   └── 12.09-nullptr.md
│
├── 13-classes-and-objects/
│   ├── 13.01-create-class.md
│   ├── 13.02-create-object.md
│   ├── 13.03-class-members.md
│   ├── 13.04-public-private-protected.md
│   ├── 13.05-member-functions.md
│   ├── 13.06-constructor.md
│   ├── 13.07-destructor.md
│   ├── 13.08-this-pointer.md
│   ├── 13.09-static-members.md
│   └── 13.10-const-member-function.md
│
├── 14-oop/
│   ├── 14.01-encapsulation.md
│   ├── 14.02-inheritance.md
│   ├── 14.03-single-inheritance.md
│   ├── 14.04-multilevel-inheritance.md
│   ├── 14.05-multiple-inheritance.md
│   ├── 14.06-hierarchical-inheritance.md
│   ├── 14.07-hybrid-inheritance.md
│   ├── 14.08-function-overriding.md
│   ├── 14.09-virtual-function.md
│   ├── 14.10-pure-virtual-function.md
│   ├── 14.11-abstract-class.md
│   ├── 14.12-polymorphism.md
│   └── 14.13-friend-function-and-class.md
│
├── 15-operator-overloading/
│   ├── 15.01-unary-operator-overloading.md
│   ├── 15.02-binary-operator-overloading.md
│   ├── 15.03-arithmetic-operator-overloading.md
│   ├── 15.04-comparison-operator-overloading.md
│   ├── 15.05-stream-insertion-overloading.md
│   ├── 15.06-stream-extraction-overloading.md
│   └── 15.07-function-call-operator.md
│
├── 16-templates/
│   ├── 16.01-function-template.md
│   ├── 16.02-class-template.md
│   ├── 16.03-multiple-template-parameters.md
│   ├── 16.04-template-specialization.md
│   ├── 16.05-template-default-arguments.md
│   └── 16.06-variadic-templates.md
│
├── 17-stl-containers/
│   ├── 17.01-vector.md
│   ├── 17.02-array.md
│   ├── 17.03-list.md
│   ├── 17.04-forward-list.md
│   ├── 17.05-deque.md
│   ├── 17.06-stack.md
│   ├── 17.07-queue.md
│   ├── 17.08-priority-queue.md
│   ├── 17.09-set.md
│   ├── 17.10-multiset.md
│   ├── 17.11-map.md
│   ├── 17.12-multimap.md
│   ├── 17.13-unordered-set.md
│   ├── 17.14-unordered-map.md
│   └── 17.15-pair.md
│
├── 18-stl-algorithms-and-iterators/
│   ├── 18.01-iterators.md
│   ├── 18.02-sort.md
│   ├── 18.03-reverse.md
│   ├── 18.04-find.md
│   ├── 18.05-count.md
│   ├── 18.06-min-max.md
│   ├── 18.07-binary-search.md
│   ├── 18.08-lower-bound.md
│   ├── 18.09-upper-bound.md
│   ├── 18.10-accumulate.md
│   ├── 18.11-transform.md
│   └── 18.12-unique.md
│
├── 19-exception-handling/
│   ├── 19.01-try-catch.md
│   ├── 19.02-throw.md
│   ├── 19.03-multiple-catch.md
│   ├── 19.04-catch-all.md
│   ├── 19.05-standard-exception.md
│   ├── 19.06-invalid-argument.md
│   ├── 19.07-out-of-range.md
│   ├── 19.08-runtime-error.md
│   ├── 19.09-nested-try.md
│   ├── 19.10-rethrow.md
│   ├── 19.11-custom-exception.md
│   ├── 19.12-exception-specification.md
│   ├── 19.13-exception-in-constructor.md
│   ├── 19.14-exception-function.md
|   ├── 19.15-exception-hierarchy.md
|   └── 19.16-exception-best-practices.md
│
├── 20-file-handling/
│   ├── 20.01-create-and-close-file.md
│   ├── 20.02-write-file.md
│   ├── 20.03-read-file.md
│   ├── 20.04-append-file.md
│   ├── 20.05-read-write-file.md
│   └── 20.06-binary-file.md
│
├── 21-memory-management/
│   ├── 21.01-new-and-delete.md
│   ├── 21.02-dynamic-array.md
│   ├── 21.03-dynamic-object.md
│   ├── 21.04-raii.md
│   ├── 21.05-smart-pointer-basics.md
│   ├── 21.06-unique-ptr.md
│   ├── 21.07-shared-ptr.md
│   ├── 21.08-weak-ptr.md
│   └── 21.09-make-unique-and-make-shared.md
│
├── 22-lambda-and-modern-cpp/
│   ├── 22.01-lambda-function.md
│   ├── 22.02-lambda-parameters.md
│   ├── 22.03-capture-list.md
│   ├── 22.04-auto-keyword.md
│   ├── 22.05-range-based-for-loop.md
│   ├── 22.06-structured-bindings.md
│   ├── 22.07-constexpr.md
│   ├── 22.08-std-optional.md
│   ├── 22.09-std-variant.md
│   ├── 22.10-std-any.md
│   ├── 22.11-move-semantics.md
│   ├── 22.12-rvalue-references.md
│   ├── 22.13-perfect-forwarding.md
│   ├── 22.14-emplace.md
│   ├── 22.15-uniform-initialization.md
│   └── 22.16-range-library.md
│
├── 23-namespaces-and-headers/
│   ├── 23.01-create-namespace.md
│   ├── 23.02-using-namespace.md
│   ├── 23.03-nested-namespace.md
│   ├── 23.04-custom-namespace.md
│   ├── 23.05-standard-headers.md
│   ├── 23.06-custom-header-file.md
│   ├── 23.07-header-guards.md
│   └── 23.08-modules.md
│
├── 24-preprocessor/
│   ├── 24.01-define.md
│   ├── 24.02-macro.md
│   ├── 24.03-conditional-compilation.md
│   ├── 24.04-include.md
│   └── 24.05-predefined-macros.md
│
├── 25-data-structures/
│   ├── 25.01-linked-list.md
│   ├── 25.02-doubly-linked-list.md
│   ├── 25.03-stack.md
│   ├── 25.04-queue.md
│   ├── 25.05-binary-tree.md
│   ├── 25.06-binary-search-tree.md
│   ├── 25.07-heap.md
│   ├── 25.08-hash-table.md
│   └── 25.09-graph.md
│
├── 26-algorithms/
│   ├── 26.01-linear-search.md
│   ├── 26.02-binary-search.md
│   ├── 26.03-bubble-sort.md
│   ├── 26.04-selection-sort.md
│   ├── 26.05-insertion-sort.md
│   ├── 26.06-merge-sort.md
│   ├── 26.07-quick-sort.md
│   ├── 26.08-recursion.md
│   ├── 26.09-divide-and-conquer.md
│   ├── 26.10-greedy-algorithm.md
│   ├── 26.11-dynamic-programming.md
│   └── 26.12-backtracking.md
│
├── 27-concurrency/
│   ├── 27.01-thread-basics.md
│   ├── 27.02-create-thread.md
│   ├── 27.03-thread-join.md
│   ├── 27.04-thread-detach.md
│   ├── 27.05-mutex.md
│   ├── 27.06-lock-guard.md
│   ├── 27.07-unique-lock.md
│   ├── 27.08-condition-variable.md
│   └── 27.09-atomic.md
│
├── 28-build-and-project-management/
│   ├── 28.01-gpp-compile.md
│   ├── 28.02-multiple-source-files.md
│   ├── 28.03-header-and-source-files.md
│   ├── 28.04-static-library.md
│   ├── 28.05-shared-library.md
│   ├── 28.06-makefile.md
│   ├── 28.07-cmake-basics.md
│   └── 28.08-cmake-project.md
│
└── 29-projects/
    ├── 29.01-calculator.md
    ├── 29.02-student-management-system.md
    ├── 29.03-library-management-system.md
    ├── 29.04-bank-management-system.md
    ├── 29.05-contact-management-system.md
    └── 29.06-file-based-crud-application.md
```

---

## Category Summary

|#|Category|Main Topics|
|---|---|---|
|**01**|**Basics**|Hello World, comments, variables, constants, data types, input/output, type casting, operators, precedence, `enum`|
|**02**|**Character**|ASCII, character input, case conversion, vowels/consonants, uppercase/lowercase, character classification|
|**03**|**Number Conversion**|Binary, decimal, octal, hexadecimal conversions and number-system operations|
|**04**|**Basic Calculation**|Sum/average, geometry, temperature conversion, swapping, mathematical operations, quadratic equation|
|**05**|**Conditions**|Even/odd, positive/negative, grading, largest/smallest number, leap year, calculator, nested conditions, ternary operator|
|**06**|**Loops & Numbers**|Tables, factorial, prime numbers, GCD/LCM, digits, palindrome, Armstrong, strong numbers, series, Fibonacci|
|**07**|**Patterns**|Number, binary, alphabetic, symbol, flow, pyramid, star, Floyd triangle and miscellaneous patterns|
|**08**|**Arrays**|Array input/output, sum/average, min/max, copy, reverse, insertion, deletion, searching, sorting, duplicates|
|**09**|**Matrices**|Matrix creation, addition, subtraction, multiplication, transpose, diagonal, triangular sums, symmetric matrix|
|**10**|**Strings**|`std::string`, input, length, concatenation, comparison, reverse, palindrome, case conversion, character counting, substring|
|**11**|**Functions & Recursion**|Functions, parameters, return values, default arguments, overloading, inline functions, arrays/strings as arguments, references, recursion|
|**12**|**Pointers & References**|Pointers, pointer arithmetic, arrays through pointers, function pointers, pointer-to-pointer, references, `nullptr`, pointer vs reference|
|**13**|**Classes & Objects**|Classes, objects, members, access specifiers, member functions, constructors, destructors, `this`, static members, `const` member functions|
|**14**|**OOP**|Encapsulation, inheritance, polymorphism, abstraction, overriding, virtual functions, pure virtual functions, abstract classes, friend functions/classes|
|**15**|**Operator Overloading**|Unary, binary, arithmetic, comparison, stream insertion/extraction, function-call operator|
|**16**|**Templates**|Function templates, class templates, multiple parameters, specialization, default arguments, variadic templates|
|**17**|**STL Containers**|`vector`, `array`, `list`, `forward_list`, `deque`, `stack`, `queue`, `priority_queue`, `set`, `map`, unordered containers, `pair`|
|**18**|**STL Algorithms & Iterators**|Iterators, sorting, searching, counting, reversing, min/max, binary search, bounds, accumulation, transformation, unique|
|**19**|**Exception Handling**|`try`, `catch`, `throw`, multiple exceptions, custom exceptions, standard exceptions|
|**20**|**File Handling**|`ifstream`, `ofstream`, `fstream`, file creation, reading, writing, appending, formatted I/O, binary files|
|**21**|**Memory Management**|`new`, `delete`, dynamic arrays, dynamic objects, RAII, smart pointers, `unique_ptr`, `shared_ptr`, `weak_ptr`, factory helpers|
|**22**|**Lambda & Modern C++**|Lambdas, capture lists, `auto`, range-based loops, structured bindings, `constexpr`, `optional`, `variant`, `any`, move semantics, rvalue references, forwarding, `emplace`, ranges|
|**23**|**Namespaces & Headers**|Namespaces, nested namespaces, `using`, standard headers, custom headers, header guards, modules|
|**24**|**Preprocessor**|`#define`, macros, conditional compilation, `#include`, predefined macros|
|**25**|**Data Structures**|Linked lists, stacks, queues, trees, binary search trees, heaps, hash tables, graphs|
|**26**|**Algorithms**|Searching, sorting, recursion, divide and conquer, greedy algorithms, dynamic programming, backtracking|
|**27**|**Concurrency**|Threads, `std::thread`, `join`, `detach`, mutexes, lock guards, unique locks, condition variables, atomic operations|
|**28**|**Build & Project Management**|`g++`, compilation, multiple source files, headers, static/shared libraries, Makefile, CMake|
|**29**|**Projects**|Calculator, student management, library management, bank management, contact management, file-based CRUD applications|

---
# Online Compiler

You can run C++ programs directly in your browser without installing a compiler.

**ZuupCode Online IDE:**  [https://code.zuup.dev/editor](https://code.zuup.dev/editor)

### Quick Test

```cpp
#include <iostream>

int main()
{
    int a = 10;
    int b = 20;

    std::cout << "Sum = " << a + b << '\n';

    return 0;
}
```

**Output:**

```text
Sum = 30
```

---

> **Organized by topic. Numbered by hierarchy. Focused on C++. Built for practical problem solving.**
