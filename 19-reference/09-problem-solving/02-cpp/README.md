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
│   ├── 01-hello-world.md
│   ├── 02-comments.md
│   ├── 03-variables.md
│   ├── 04-constants.md
│   ├── 05-data-types.md
│   ├── 06-input-output.md
│   ├── 07-type-casting.md
│   ├── 08-operators.md
│   ├── 09-operator-precedence.md
│   └── 10-enumeration.md
│
├── 02-character/
│   ├── 01-find-ascii-value.md
│   ├── 02-lowercase-to-uppercase.md
│   ├── 03-uppercase-to-lowercase.md
│   ├── 04-lowercase-to-uppercase-using-function.md
│   ├── 05-uppercase-to-lowercase-using-function.md
│   ├── 06-vowel-or-consonant.md
│   ├── 07-capital-or-small-letter.md
│   └── 08-character-classification.md
│
├── 03-number-conversion/
│   ├── 01-decimal-to-binary.md
│   ├── 02-binary-to-decimal.md
│   ├── 03-decimal-to-octal.md
│   ├── 04-octal-to-decimal.md
│   ├── 05-decimal-to-hexadecimal.md
│   ├── 06-hexadecimal-to-decimal.md
│   ├── 07-binary-to-octal.md
│   ├── 08-octal-to-binary.md
│   ├── 09-binary-to-hexadecimal.md
│   ├── 10-hexadecimal-to-binary.md
│   ├── 11-octal-to-hexadecimal.md
│   └── 12-hexadecimal-to-octal.md
│
├── 04-basic-calculation/
│   ├── 01-summation-and-average.md
│   ├── 02-area-of-triangle.md
│   ├── 03-area-of-triangle-from-three-values.md
│   ├── 04-area-of-rectangle.md
│   ├── 05-area-of-circle.md
│   ├── 06-celsius-to-fahrenheit.md
│   ├── 07-fahrenheit-to-celsius.md
│   ├── 08-swap-with-temporary-variable.md
│   ├── 09-swap-without-temporary-variable.md
│   └── 10-quadratic-equation.md
│
├── 05-conditions/
│   ├── 01-even-or-odd.md
│   ├── 02-positive-or-negative.md
│   ├── 03-student-grade-system.md
│   ├── 04-largest-of-three-numbers.md
│   ├── 05-smallest-of-three-numbers.md
│   ├── 06-leap-year.md
│   ├── 07-calculator-using-switch.md
│   ├── 08-nested-if.md
│   └── 09-ternary-operator.md
│
├── 06-loops-and-numbers/
│   ├── 01-multiplication-table.md
│   ├── 02-factorial.md
│   ├── 03-prime-number.md
│   ├── 04-prime-numbers-in-range.md
│   ├── 05-gcd-and-lcm.md
│   ├── 06-sum-of-digits.md
│   ├── 07-reverse-number.md
│   ├── 08-palindrome-number.md
│   ├── 09-armstrong-number.md
│   ├── 10-armstrong-numbers-in-range.md
│   ├── 11-count-digit-in-integer.md
│   ├── 12-strong-number.md
│   ├── 13-fibonacci-series.md
│   ├── 14-natural-number-series-sum.md
│   ├── 15-even-number-series-sum.md
│   ├── 16-odd-number-series-sum.md
│   ├── 17-square-series-sum.md
│   ├── 18-cube-series-sum.md
│   ├── 19-harmonic-series-sum.md
│   └── 20-alternating-series-sum.md
│
├── 07-patterns/
│   ├── 01-number-triangles/
│   │   ├── 01-right-angle-triangle-01.md
│   │   ├── 02-right-angle-triangle-02.md
│   │   ├── 03-right-angle-triangle-03.md
│   │   ├── 04-right-angle-triangle-04.md
│   │   ├── 05-right-angle-triangle-05.md
│   │   ├── 06-right-angle-triangle-06.md
│   │   ├── 07-left-angle-triangle-01.md
│   │   ├── 08-left-angle-triangle-02.md
│   │   ├── 09-left-angle-triangle-03.md
│   │   ├── 10-left-angle-triangle-04.md
│   │   ├── 11-left-angle-triangle-05.md
│   │   └── 12-left-angle-triangle-06.md
│   │
│   ├── 02-binary-triangles/
│   │   ├── 01-right-angle-triangle-01.md
│   │   ├── 02-right-angle-triangle-02.md
│   │   ├── 03-right-angle-triangle-03.md
│   │   ├── 04-right-angle-triangle-04.md
│   │   ├── 05-right-angle-triangle-05.md
│   │   ├── 06-right-angle-triangle-06.md
│   │   ├── 07-left-angle-triangle-01.md
│   │   ├── 08-left-angle-triangle-02.md
│   │   ├── 09-left-angle-triangle-03.md
│   │   ├── 10-left-angle-triangle-04.md
│   │   ├── 11-left-angle-triangle-05.md
│   │   └── 12-left-angle-triangle-06.md
│   │
│   ├── 03-alphabetic-triangles/
│   │   ├── 01-right-angle-triangle-01.md
│   │   ├── 02-right-angle-triangle-02.md
│   │   ├── 03-right-angle-triangle-03.md
│   │   ├── 04-right-angle-triangle-04.md
│   │   ├── 05-right-angle-triangle-05.md
│   │   ├── 06-right-angle-triangle-06.md
│   │   ├── 07-left-angle-triangle-01.md
│   │   ├── 08-left-angle-triangle-02.md
│   │   ├── 09-left-angle-triangle-03.md
│   │   ├── 10-left-angle-triangle-04.md
│   │   ├── 11-left-angle-triangle-05.md
│   │   └── 12-left-angle-triangle-06.md
│   │
│   ├── 04-symbol-triangles/
│   │   ├── 01-right-angle-triangle-01.md
│   │   ├── 02-right-angle-triangle-02.md
│   │   ├── 03-right-angle-triangle-03.md
│   │   ├── 04-right-angle-triangle-04.md
│   │   ├── 05-right-angle-triangle-05.md
│   │   ├── 06-right-angle-triangle-06.md
│   │   ├── 07-left-angle-triangle-01.md
│   │   ├── 08-left-angle-triangle-02.md
│   │   └── 09-left-angle-triangle-03.md
│   │
│   ├── 05-flow-patterns/
│   │   ├── 01-pattern-01.md
│   │   ├── 02-pattern-02.md
│   │   ├── 03-pattern-03.md
│   │   ├── 04-pattern-04.md
│   │   └── 05-pattern-05.md
│   │
│   ├── 06-pyramid-patterns/
│   │   ├── 01-pyramid-01.md
│   │   ├── 02-pyramid-02.md
│   │   ├── 03-pyramid-03.md
│   │   ├── 04-pyramid-04.md
│   │   ├── 05-pyramid-05.md
│   │   ├── 06-pyramid-06.md
│   │   ├── 07-pattern-07.md
│   │   ├── 08-pattern-08.md
│   │   ├── 09-pattern-09.md
│   │   ├── 10-pyramid-10.md
│   │   ├── 11-pattern-11.md
│   │   ├── 12-pattern-12.md
│   │   ├── 13-pyramid-13.md
│   │   ├── 14-pattern-14.md
│   │   ├── 15-pattern-15.md
│   │   └── 16-pyramid-16.md
│   │
│   ├── 07-rectangle-star.md
│   ├── 08-triangle-star.md
│   ├── 09-x-shape-star.md
│   └── 10-floyd-triangle.md
│
├── 08-arrays/
│   ├── 01-input-array.md
│   ├── 02-print-array.md
│   ├── 03-summation-and-average.md
│   ├── 04-maximum-and-minimum.md
│   ├── 05-copy-array.md
│   ├── 06-reverse-array.md
│   ├── 07-delete-array-element.md
│   ├── 08-insert-array-element.md
│   ├── 09-linear-search.md
│   ├── 10-binary-search.md
│   ├── 11-sort-array.md
│   └── 12-remove-duplicates.md
│
├── 09-matrices/
│   ├── 01-create-matrix.md
│   ├── 02-matrix-addition.md
│   ├── 03-matrix-subtraction.md
│   ├── 04-matrix-multiplication.md
│   ├── 05-transpose-matrix.md
│   ├── 06-diagonal-sum.md
│   ├── 07-upper-and-lower-triangle-sum.md
│   └── 08-symmetric-matrix.md
│
├── 10-strings/
│   ├── 01-string-input.md
│   ├── 02-string-length.md
│   ├── 03-string-concatenation.md
│   ├── 04-string-comparison.md
│   ├── 05-reverse-string.md
│   ├── 06-string-palindrome.md
│   ├── 07-string-uppercase-lowercase.md
│   ├── 08-count-vowels-consonants-digits-words.md
│   ├── 09-count-capital-small-letters-digits.md
│   ├── 10-substring-search.md
│   └── 11-string-tokenization.md
│
├── 11-functions/
│   ├── 01-function-definition.md
│   ├── 02-function-parameters.md
│   ├── 03-return-value.md
│   ├── 04-default-arguments.md
│   ├── 05-function-overloading.md
│   ├── 06-inline-function.md
│   ├── 07-passing-array-to-function.md
│   ├── 08-passing-string-to-function.md
│   ├── 09-reference-parameters.md
│   └── 10-recursive-function.md
│
├── 12-pointers-and-references/
│   ├── 01-pointer-basics.md
│   ├── 02-pointer-to-variable.md
│   ├── 03-pointer-arithmetic.md
│   ├── 04-pointer-and-array.md
│   ├── 05-pointer-to-function.md
│   ├── 06-pointer-to-pointer.md
│   ├── 07-reference-variable.md
│   ├── 08-pointer-vs-reference.md
│   └── 09-nullptr.md
│
├── 13-classes-and-objects/
│   ├── 01-create-class.md
│   ├── 02-create-object.md
│   ├── 03-class-members.md
│   ├── 04-public-private-protected.md
│   ├── 05-member-functions.md
│   ├── 06-constructor.md
│   ├── 07-destructor.md
│   ├── 08-this-pointer.md
│   ├── 09-static-members.md
│   └── 10-const-member-function.md
│
├── 14-oop/
│   ├── 01-encapsulation.md
│   ├── 02-inheritance.md
│   ├── 03-single-inheritance.md
│   ├── 04-multilevel-inheritance.md
│   ├── 05-multiple-inheritance.md
│   ├── 06-hierarchical-inheritance.md
│   ├── 07-hybrid-inheritance.md
│   ├── 08-function-overriding.md
│   ├── 09-virtual-function.md
│   ├── 10-pure-virtual-function.md
│   ├── 11-abstract-class.md
│   ├── 12-polymorphism.md
│   └── 13-friend-function-and-class.md
│
├── 15-operator-overloading/
│   ├── 01-unary-operator-overloading.md
│   ├── 02-binary-operator-overloading.md
│   ├── 03-arithmetic-operator-overloading.md
│   ├── 04-comparison-operator-overloading.md
│   ├── 05-stream-insertion-overloading.md
│   ├── 06-stream-extraction-overloading.md
│   └── 07-function-call-operator.md
│
├── 16-templates/
│   ├── 01-function-template.md
│   ├── 02-class-template.md
│   ├── 03-multiple-template-parameters.md
│   ├── 04-template-specialization.md
│   ├── 05-template-default-arguments.md
│   └── 06-variadic-templates.md
│
├── 17-stl-containers/
│   ├── 01-vector.md
│   ├── 02-array.md
│   ├── 03-list.md
│   ├── 04-forward-list.md
│   ├── 05-deque.md
│   ├── 06-stack.md
│   ├── 07-queue.md
│   ├── 08-priority-queue.md
│   ├── 09-set.md
│   ├── 10-multiset.md
│   ├── 11-map.md
│   ├── 12-multimap.md
│   ├── 13-unordered-set.md
│   ├── 14-unordered-map.md
│   └── 15-pair.md
│
├── 18-stl-algorithms-and-iterators/
│   ├── 01-iterators.md
│   ├── 02-sort.md
│   ├── 03-reverse.md
│   ├── 04-find.md
│   ├── 05-count.md
│   ├── 06-min-max.md
│   ├── 07-binary-search.md
│   ├── 08-lower-bound.md
│   ├── 09-upper-bound.md
│   ├── 10-accumulate.md
│   ├── 11-transform.md
│   └── 12-unique.md
│
├── 19-exception-handling/
│   ├── 01-try-catch.md
│   ├── 02-throw.md
│   ├── 03-multiple-catch.md
│   ├── 04-catch-all.md
│   ├── 05-standard-exception.md
│   ├── 06-invalid-argument.md
│   ├── 07-out-of-range.md
│   ├── 08-runtime-error.md
│   ├── 09-nested-try.md
│   ├── 10-rethrow.md
│   ├── 11-custom-exception.md
│   ├── 12-exception-specification.md
│   ├── 13-exception-in-constructor.md
│   ├── 14-exception-function.md
│   ├── 15-exception-hierarchy.md
│   └── 16-exception-best-practices.md
│
├── 20-file-handling/
│   ├── 01-create-and-close-file.md
│   ├── 02-write-file.md
│   ├── 03-read-file.md
│   ├── 04-append-file.md
│   ├── 05-read-write-file.md
│   └── 06-binary-file.md
│
├── 21-memory-management/
│   ├── 01-new-and-delete.md
│   ├── 02-dynamic-array.md
│   ├── 03-dynamic-object.md
│   ├── 04-raii.md
│   ├── 05-smart-pointer-basics.md
│   ├── 06-unique-ptr.md
│   ├── 07-shared-ptr.md
│   ├── 08-weak-ptr.md
│   └── 09-make-unique-and-make-shared.md
│
├── 22-lambda-and-modern-cpp/
│   ├── 01-lambda-function.md
│   ├── 02-lambda-parameters.md
│   ├── 03-capture-list.md
│   ├── 04-auto-keyword.md
│   ├── 05-range-based-for-loop.md
│   ├── 06-structured-bindings.md
│   ├── 07-constexpr.md
│   ├── 08-std-optional.md
│   ├── 09-std-variant.md
│   ├── 10-std-any.md
│   ├── 11-move-semantics.md
│   ├── 12-rvalue-references.md
│   ├── 13-perfect-forwarding.md
│   ├── 14-emplace.md
│   ├── 15-uniform-initialization.md
│   └── 16-range-library.md
│
├── 23-namespaces-and-headers/
│   ├── 01-create-namespace.md
│   ├── 02-using-namespace.md
│   ├── 03-nested-namespace.md
│   ├── 04-custom-namespace.md
│   ├── 05-standard-headers.md
│   ├── 06-custom-header-file.md
│   ├── 07-header-guards.md
│   └── 08-modules.md
│
├── 24-preprocessor/
│   ├── 01-define.md
│   ├── 02-macro.md
│   ├── 03-conditional-compilation.md
│   ├── 04-include.md
│   └── 05-predefined-macros.md
│
├── 25-data-structures/
│   ├── 01-linked-list.md
│   ├── 02-doubly-linked-list.md
│   ├── 03-stack.md
│   ├── 04-queue.md
│   ├── 05-binary-tree.md
│   ├── 06-binary-search-tree.md
│   ├── 07-heap.md
│   ├── 08-hash-table.md
│   └── 09-graph.md
│
├── 26-algorithms/
│   ├── 01-linear-search.md
│   ├── 02-binary-search.md
│   ├── 03-bubble-sort.md
│   ├── 04-selection-sort.md
│   ├── 05-insertion-sort.md
│   ├── 06-merge-sort.md
│   ├── 07-quick-sort.md
│   ├── 08-recursion.md
│   ├── 09-divide-and-conquer.md
│   ├── 10-greedy-algorithm.md
│   ├── 11-dynamic-programming.md
│   └── 12-backtracking.md
│
├── 27-concurrency/
│   ├── 01-thread-basics.md
│   ├── 02-create-thread.md
│   ├── 03-thread-join.md
│   ├── 04-thread-detach.md
│   ├── 05-mutex.md
│   ├── 06-lock-guard.md
│   ├── 07-unique-lock.md
│   ├── 08-condition-variable.md
│   └── 09-atomic.md
│
├── 28-build-and-project-management/
│   ├── 01-gpp-compile.md
│   ├── 02-multiple-source-files.md
│   ├── 03-header-and-source-files.md
│   ├── 04-static-library.md
│   ├── 05-shared-library.md
│   ├── 06-makefile.md
│   ├── 07-cmake-basics.md
│   └── 08-cmake-project.md
│
└── 29-projects/
    ├── 01-calculator.md
    ├── 02-student-management-system.md
    ├── 03-library-management-system.md
    ├── 04-bank-management-system.md
    ├── 05-contact-management-system.md
    └── 06-file-based-crud-application.md
```

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
