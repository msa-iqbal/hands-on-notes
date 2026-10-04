# C Problem Solving

> C programming problems organized by topic and learning sequence.

## Directory Tree

```text
01-c/
│
├── README.md
│
├── 01-character/
│   ├── 01.01-find-ascii-value.md
│   ├── 01.02-find-ascii-value.md
│   ├── 01.03-lowercase-to-uppercase.md
│   ├── 01.04-uppercase-to-lowercase.md
│   ├── 01.05-lowercase-to-uppercase-using-library-function.md
│   ├── 01.06-uppercase-to-lowercase-using-library-function.md
│   ├── 01.07-vowel-or-consonant.md
│   └── 01.08-capital-or-small-letter.md
│
├── 02-number-conversion/
│   ├── 02.01-decimal-to-octal.md
│   ├── 02.02-octal-to-decimal.md
│   ├── 02.03-decimal-to-hexadecimal.md
│   ├── 02.04-hexadecimal-to-decimal.md
│   ├── 02.05-octal-to-hexadecimal.md
│   ├── 02.06-hexadecimal-to-octal.md
│   └── 02.07-number-conversions-using-switch.md
│
├── 03-basic-calculation/
│   ├── 03.01-summation-and-average.md
│   ├── 03.02-area-of-triangle.md
│   ├── 03.03-area-of-triangle-from-three-values.md
│   ├── 03.04-area-of-rectangle.md
│   ├── 03.05-area-of-circle.md
│   ├── 03.06-celsius-to-fahrenheit.md
│   ├── 03.07-fahrenheit-to-celsius.md
│   ├── 03.08-swap-with-temporary-variable.md
│   ├── 03.09-swap-without-temporary-variable.md
│   └── 03.10-quadratic-equation.md
│
├── 04-conditions/
│   ├── 04.01-even-or-odd.md
│   ├── 04.02-positive-or-negative.md
│   ├── 04.03-student-grade-system.md
│   ├── 04.04-largest-of-three-numbers.md
│   ├── 04.05-leap-year.md
│   └── 04.06-calculator-using-switch.md
│
├── 05-loops-and-numbers/
│   ├── 05.01-multiplication-table.md
│   ├── 05.02-factorial.md
│   ├── 05.03-prime-number.md
│   ├── 05.04-gcd-and-lcm.md
│   ├── 05.05-sum-of-digits.md
│   ├── 05.06-reverse-number.md
│   ├── 05.07-palindrome-number.md
│   ├── 05.08-armstrong-number.md
│   ├── 05.09-armstrong-numbers-in-range.md
│   ├── 05.10-count-digit-in-integer.md
│   ├── 05.11-strong-number.md
│   ├── 05.12-odd-number-series-sum-01.md
│   ├── 05.13-odd-number-series-sum-02.md
│   ├── 05.14-product-series.md
│   ├── 05.15-natural-number-series-sum.md
│   ├── 05.16-fractional-series-sum.md
│   ├── 05.17-natural-number-series-product.md
│   ├── 05.18-squared-number-series-product.md
│   ├── 05.19-odd-number-series-sum-03.md
│   ├── 05.20-odd-number-series-product.md
│   ├── 05.21-squared-odd-series-product.md
│   ├── 05.22-even-number-series-sum.md
│   ├── 05.23-even-number-series-product.md
│   ├── 05.24-squared-even-series-product.md
│   ├── 05.25-square-series-sum.md
│   ├── 05.26-harmonic-series-sum.md
│   ├── 05.27-alternating-series-sum.md
│   └── 05.28-fibonacci-series.md
│
├── 06-patterns/
│   │
│   ├── 06.01-number-triangles/
│   │   ├── 06.01.01-right-angle-triangle-01.md
│   │   ├── 06.01.02-right-angle-triangle-02.md
│   │   ├── 06.01.03-right-angle-triangle-03.md
│   │   ├── 06.01.04-right-angle-triangle-04.md
│   │   ├── 06.01.05-right-angle-triangle-05.md
│   │   ├── 06.01.06-right-angle-triangle-06.md
│   │   ├── 06.01.07-left-angle-triangle-01.md
│   │   ├── 06.01.08-left-angle-triangle-02.md
│   │   ├── 06.01.09-left-angle-triangle-03.md
│   │   ├── 06.01.10-left-angle-triangle-04.md
│   │   ├── 06.01.11-left-angle-triangle-05.md
│   │   └── 06.01.12-left-angle-triangle-06.md
│   │
│   ├── 06.02-binary-triangles/
│   │   ├── 06.02.01-right-angle-triangle-01.md
│   │   ├── 06.02.02-right-angle-triangle-02.md
│   │   ├── 06.02.03-right-angle-triangle-03.md
│   │   ├── 06.02.04-right-angle-triangle-04.md
│   │   ├── 06.02.05-right-angle-triangle-05.md
│   │   ├── 06.02.06-right-angle-triangle-06.md
│   │   ├── 06.02.07-left-angle-triangle-01.md
│   │   ├── 06.02.08-left-angle-triangle-02.md
│   │   ├── 06.02.09-left-angle-triangle-03.md
│   │   ├── 06.02.10-left-angle-triangle-04.md
│   │   ├── 06.02.11-left-angle-triangle-05.md
│   │   └── 06.02.12-left-angle-triangle-06.md
│   │
│   ├── 06.03-alphabetic-triangles/
│   │   ├── 06.03.01-right-angle-triangle-01.md
│   │   ├── 06.03.02-right-angle-triangle-02.md
│   │   ├── 06.03.03-right-angle-triangle-03.md
│   │   ├── 06.03.04-right-angle-triangle-04.md
│   │   ├── 06.03.05-right-angle-triangle-05.md
│   │   ├── 06.03.06-right-angle-triangle-06.md
│   │   ├── 06.03.07-left-angle-triangle-01.md
│   │   ├── 06.03.08-left-angle-triangle-02.md
│   │   ├── 06.03.09-left-angle-triangle-03.md
│   │   ├── 06.03.10-left-angle-triangle-04.md
│   │   ├── 06.03.11-left-angle-triangle-05.md
│   │   └── 06.03.12-left-angle-triangle-06.md
│   │
│   ├── 06.04-symbol-triangles/
│   │   ├── 06.04.01-right-angle-triangle-01.md
│   │   ├── 06.04.02-right-angle-triangle-02.md
│   │   ├── 06.04.03-right-angle-triangle-03.md
│   │   ├── 06.04.04-right-angle-triangle-04.md
│   │   ├── 06.04.05-right-angle-triangle-05.md
│   │   ├── 06.04.06-right-angle-triangle-06.md
│   │   ├── 06.04.07-left-angle-triangle-01.md
│   │   ├── 06.04.08-left-angle-triangle-02.md
│   │   └── 06.04.09-left-angle-triangle-03.md
│   │
│   ├── 06.05-alphabetic-left-right-triangle.md
│   │
│   ├── 06.06-flow-patterns/
│   │   ├── 06.06.01-pattern-01.md
│   │   ├── 06.06.02-pattern-02.md
│   │   ├── 06.06.03-pattern-03.md
│   │   ├── 06.06.04-pattern-04.md
│   │   └── 06.06.05-pattern-05.md
│   │
│   ├── 06.07-pattern.md
│   │
│   ├── 06.08-pyramid-patterns/
│   │   ├── 06.08.01-pyramid-01.md
│   │   ├── 06.08.02-pattern-02.md
│   │   ├── 06.08.03-pattern-03.md
│   │   ├── 06.08.04-pyramid-04.md
│   │   ├── 06.08.05-pyramid-05.md
│   │   ├── 06.08.06-pyramid-06.md
│   │   ├── 06.08.07-pattern-07.md
│   │   ├── 06.08.08-pattern-08.md
│   │   ├── 06.08.09-pattern-09.md
│   │   ├── 06.08.10-pyramid-10.md
│   │   ├── 06.08.11-pattern-11.md
│   │   ├── 06.08.12-pattern-12.md
│   │   ├── 06.08.13-pyramid-13.md
│   │   ├── 06.08.14-pattern-14.md
│   │   ├── 06.08.15-pattern-15.md
│   │   ├── 06.08.16-pyramid-16.md
│   │   ├── 06.08.17-pattern-17.md
│   │   ├── 06.08.18-pyramid-18.md
│   │   ├── 06.08.19-pattern-19.md
│   │   ├── 06.08.20-pyramid-20.md
│   │   ├── 06.08.21-pattern-21.md
│   │   ├── 06.08.22-pyramid-22.md
│   │   └── 06.08.23-pattern-23.md
│   │
│   ├── 06.09-rectangle-star.md
│   ├── 06.10-triangle-star.md
│   ├── 06.11-x-shape-star.md
│   ├── 06.12-floyd-triangle.md
│   │
│   └── 06.13-patterns/
│       ├── 06.13.01-pattern-01.md
│       ├── 06.13.02-pattern-02.md
│       └── 06.13.03-pattern-03.md
│
├── 07-arrays/
│   ├── 07.01-summation-and-average.md
│   ├── 07.02-maximum-and-minimum.md
│   ├── 07.03-fibonacci-using-array.md
│   ├── 07.04-copy-array.md
│   ├── 07.05-delete-array-element.md
│   └── 07.06-linear-search.md
│
├── 08-matrix/
│   ├── 08.01-create-matrix.md
│   ├── 08.02-matrix-addition-and-subtraction.md
│   ├── 08.03-matrix-multiplication.md
│   ├── 08.04-transpose-matrix.md
│   ├── 08.05-diagonal-sum.md
│   └── 08.06-upper-and-lower-triangle-sum.md
│
├── 09-strings/
│   ├── 09.01-print-string-characters.md
│   ├── 09.02-string-length-without-function.md
│   ├── 09.03-string-concatenation-without-strcat.md
│   ├── 09.04-reverse-string-using-strrev.md
│   ├── 09.05-string-palindrome.md
│   ├── 09.06-string-swapping.md
│   ├── 09.07-string-uppercase-lowercase.md
│   ├── 09.08-count-vowels-consonants-digits-words.md
│   └── 09.09-count-capital-small-letters-digits.md
│
├── 10-functions-and-recursion/
│   ├── 10.01-area-of-triangle-using-function.md
│   ├── 10.02-power-using-function.md
│   ├── 10.03-passing-array-to-function.md
│   ├── 10.04-maximum-array-value-using-function.md
│   ├── 10.05-passing-string-to-function.md
│   └── 10.06-factorial-using-recursion.md
│
├── 11-structures-and-unions/
│   ├── 11.01-input-structure-elements.md
│   ├── 11.02-initialize-structure-variables.md
│   ├── 11.03-structure-comparison.md
│   ├── 11.04-array-of-structures.md
│   ├── 11.05-structure-array-within-structure.md
│   ├── 11.06-passing-structure-to-function.md
│   ├── 11.07-size-of-union-and-structure.md
│   ├── 11.08-typedef-primitive-data-types.md
│   └── 11.09-typedef-user-defined-data-types.md
│
├── 12-pointers/
│   ├── 12.01-pointer-to-different-variables.md
│   ├── 12.02-add-numbers-using-pointer.md
│   ├── 12.03-swap-using-pointer.md
│   ├── 12.04-swap-using-pointer-and-function.md
│   └── 12.05-array-elements-using-pointer.md
│
├── 13-file-handling/
│   ├── 13.01-file-create-and-close.md
│   ├── 13.02-write-file-using-fputc.md
│   ├── 13.03-write-file-using-fputs.md
│   ├── 13.04-write-file-using-fprintf.md
│   ├── 13.05-write-student-details-using-fprintf.md
│   ├── 13.06-read-file-using-fgetc.md
│   ├── 13.07-read-file-using-fgets.md
│   └── 13.08-read-file-using-fscanf.md
│
└── 14-preprocessor/
    ├── 14.01-define-preprocessor.md
    └── 14.02-user-defined-header-file.md
```

---

## Category Summary

| #      | Category                  | Main Topics                                                                                           |
| ------ | ------------------------- | ----------------------------------------------------------------------------------------------------- |
| **01** | **Character**             | ASCII, case conversion, vowels/consonants, uppercase/lowercase                                        |
| **02** | **Number Conversion**     | Decimal, octal, hexadecimal conversions, switch-based conversion                                      |
| **03** | **Basic Calculation**     | Sum/average, geometry, temperature conversion, swapping, quadratic equation                           |
| **04** | **Conditions**            | Even/odd, positive/negative, grading, largest number, leap year, calculator                           |
| **05** | **Loops & Numbers**       | Tables, factorial, prime, GCD/LCM, digits, palindrome, Armstrong, special numbers, series, Fibonacci  |
| **06** | **Patterns**              | Number, binary, alphabetic, symbol, flow, pyramid, star, Floyd, and miscellaneous patterns            |
| **07** | **Arrays**                | Sum/average, min/max, Fibonacci, copying, deletion, linear search                                     |
| **08** | **Matrix**                | Matrix creation, arithmetic, multiplication, transpose, diagonal, triangular sums                     |
| **09** | **Strings**               | Characters, length, concatenation, reverse, palindrome, swapping, case conversion, character counting |
| **10** | **Functions & Recursion** | Functions, arrays/strings as arguments, maximum value, recursion, factorial                           |
| **11** | **Structures & Unions**   | Structure input, initialization, comparison, arrays, nested structures, functions, unions, typedef    |
| **12** | **Pointers**              | Pointer variables, arithmetic, swapping, functions, arrays through pointers                           |
| **13** | **File Handling**         | File creation, writing, reading, `fputc`, `fputs`, `fprintf`, `fgetc`, `fgets`, `fscanf`              |
| **14** | **Preprocessor**          | `#define`, user-defined header files                                                                  |

---

## Online Compiler

You can run the C programs directly in your browser without installing a C compiler.

**ZuupCode Online IDE:**  [https://code.zuup.dev/editor](https://code.zuup.dev/editor)
### Quick Test

```c
#include <stdio.h>

int main(void)
{
    int a = 10;
    int b = 20;

    printf("Sum = %d\n", a + b);

    return 0;
}
```

**Output:**

```text
Sum = 30
```

---

> **Organized by topic. Numbered by hierarchy. Easy to extend.**
