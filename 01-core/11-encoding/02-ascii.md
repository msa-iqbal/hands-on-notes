# ASCII

ASCII stands for **American Standard Code for Information Interchange**.

It is an early character encoding standard developed to represent English letters, decimal digits, punctuation marks, and control characters in computers and communication systems.

ASCII uses **7 bits** to represent each character, providing 128 possible character values.

ASCII is important because it influenced many modern character encoding systems, including UTF-8.

# 🗁 What Is ASCII?

ASCII assigns a numerical value to each supported character.

For example:

```text
Character: A

Decimal:     65
Binary:      01000001
Hexadecimal: 41
```

Another example:

```text
Character: a

Decimal:     97
Binary:      01100001
Hexadecimal: 61
```

Each ASCII character has a defined numerical representation.

# 🗁 ASCII Bit Structure

ASCII uses seven bits per character.

The number of possible combinations is:

\[
2^7 = 128
\]

Therefore, ASCII supports values from 0 to 127.

```text
Minimum value: 0
Maximum value: 127
Total values: 128
```

Although ASCII uses seven bits, it is commonly stored in an eight-bit byte with the most significant bit set to zero.

Example:

```text
ASCII value: 65

7-bit:
1000001

8-bit byte:
01000001
```

# 🗁 ASCII Character Categories

The original ASCII character set contains 128 values divided into two main categories.

| Range | Category | Description |
| --- | --- | --- |
| 0–31 | Control characters | Control communication and text processing |
| 32–126 | Printable characters | Letters, digits, punctuation, and symbols |
| 127 | Control character | Delete (DEL) |

Printable characters include:

* Uppercase letters.
* Lowercase letters.
* Decimal digits.
* Punctuation marks.
* Special symbols.
* Space character.

# 🗁 ASCII Control Characters

Control characters are non-printable characters used to control devices, communication, and text processing.

Important ASCII control characters include:

| Decimal | Hex | Name | Abbreviation |
| ---: | --- | --- | --- |
| 0 | `00` | Null | NUL |
| 7 | `07` | Bell | BEL |
| 8 | `08` | Backspace | BS |
| 9 | `09` | Horizontal Tab | HT |
| 10 | `0A` | Line Feed | LF |
| 11 | `0B` | Vertical Tab | VT |
| 12 | `0C` | Form Feed | FF |
| 13 | `0D` | Carriage Return | CR |
| 27 | `1B` | Escape | ESC |
| 127 | `7F` | Delete | DEL |

These characters are not normally displayed as visible symbols.

# 🗁 ASCII Printable Characters

Printable ASCII characters range from decimal 32 to 126.

Important ranges:

| Decimal Range | Characters |
| --- | --- |
| 32 | Space |
| 33–47 | Punctuation and symbols |
| 48–57 | Digits `0–9` |
| 58–64 | Punctuation and symbols |
| 65–90 | Uppercase letters `A–Z` |
| 91–96 | Punctuation and symbols |
| 97–122 | Lowercase letters `a–z` |
| 123–126 | Punctuation and symbols |

# 🗁 ASCII Uppercase Letters

Uppercase English letters have decimal values from 65 to 90.

| Character | Decimal | Hexadecimal | Binary |
| --- | ---: | --- | --- |
| A | 65 | `41` | `01000001` |
| B | 66 | `42` | `01000010` |
| C | 67 | `43` | `01000011` |
| D | 68 | `44` | `01000100` |
| E | 69 | `45` | `01000101` |
| F | 70 | `46` | `01000110` |
| G | 71 | `47` | `01000111` |
| H | 72 | `48` | `01001000` |
| I | 73 | `49` | `01001001` |
| J | 74 | `4A` | `01001010` |
| K | 75 | `4B` | `01001011` |
| L | 76 | `4C` | `01001100` |
| M | 77 | `4D` | `01001101` |
| N | 78 | `4E` | `01001110` |
| O | 79 | `4F` | `01001111` |
| P | 80 | `50` | `01010000` |
| Q | 81 | `51` | `01010001` |
| R | 82 | `52` | `01010010` |
| S | 83 | `53` | `01010011` |
| T | 84 | `54` | `01010100` |
| U | 85 | `55` | `01010101` |
| V | 86 | `56` | `01010110` |
| W | 87 | `57` | `01010111` |
| X | 88 | `58` | `01011000` |
| Y | 89 | `59` | `01011001` |
| Z | 90 | `5A` | `01011010` |

# 🗁 ASCII Lowercase Letters

Lowercase English letters have decimal values from 97 to 122.

| Character | Decimal | Hexadecimal | Binary |
| --- | ---: | --- | --- |
| a | 97 | `61` | `01100001` |
| b | 98 | `62` | `01100010` |
| c | 99 | `63` | `01100011` |
| d | 100 | `64` | `01100100` |
| e | 101 | `65` | `01100101` |
| f | 102 | `66` | `01100110` |
| g | 103 | `67` | `01100111` |
| h | 104 | `68` | `01101000` |
| i | 105 | `69` | `01101001` |
| j | 106 | `6A` | `01101010` |
| k | 107 | `6B` | `01101011` |
| l | 108 | `6C` | `01101100` |
| m | 109 | `6D` | `01101101` |
| n | 110 | `6E` | `01101110` |
| o | 111 | `6F` | `01101111` |
| p | 112 | `70` | `01110000` |
| q | 113 | `71` | `01110001` |
| r | 114 | `72` | `01110010` |
| s | 115 | `73` | `01110011` |
| t | 116 | `74` | `01110100` |
| u | 117 | `75` | `01110101` |
| v | 118 | `76` | `01110110` |
| w | 119 | `77` | `01110111` |
| x | 120 | `78` | `01111000` |
| y | 121 | `79` | `01111001` |
| z | 122 | `7A` | `01111010` |

# 🗁 ASCII Numeric Characters

Digits `0–9` have decimal values from 48 to 57.

| Character | Decimal | Hexadecimal | Binary |
| --- | ---: | --- | --- |
| 0 | 48 | `30` | `00110000` |
| 1 | 49 | `31` | `00110001` |
| 2 | 50 | `32` | `00110010` |
| 3 | 51 | `33` | `00110011` |
| 4 | 52 | `34` | `00110100` |
| 5 | 53 | `35` | `00110101` |
| 6 | 54 | `36` | `00110110` |
| 7 | 55 | `37` | `00110111` |
| 8 | 56 | `38` | `00111000` |
| 9 | 57 | `39` | `00111001` |

# 🗁 ASCII Special Characters

ASCII includes punctuation and symbols used in programming, communication, and text processing.

| Character | Name | Decimal | Hex |
| --- | --- | ---: | --- |
| Space | Space | 32 | `20` |
| `!` | Exclamation mark | 33 | `21` |
| `"` | Double quotation mark | 34 | `22` |
| `#` | Number sign | 35 | `23` |
| `$` | Dollar sign | 36 | `24` |
| `%` | Percent sign | 37 | `25` |
| `&` | Ampersand | 38 | `26` |
| `'` | Apostrophe | 39 | `27` |
| `(` | Left parenthesis | 40 | `28` |
| `)` | Right parenthesis | 41 | `29` |
| `*` | Asterisk | 42 | `2A` |
| `+` | Plus sign | 43 | `2B` |
| `,` | Comma | 44 | `2C` |
| `-` | Hyphen-minus | 45 | `2D` |
| `.` | Period | 46 | `2E` |
| `/` | Slash | 47 | `2F` |
| `:` | Colon | 58 | `3A` |
| `;` | Semicolon | 59 | `3B` |
| `<` | Less-than sign | 60 | `3C` |
| `=` | Equals sign | 61 | `3D` |
| `>` | Greater-than sign | 62 | `3E` |
| `?` | Question mark | 63 | `3F` |
| `@` | At sign | 64 | `40` |
| `[` | Left bracket | 91 | `5B` |
| `\` | Backslash | 92 | `5C` |
| `]` | Right bracket | 93 | `5D` |
| `^` | Caret | 94 | `5E` |
| `_` | Underscore | 95 | `5F` |
| `` ` `` | Grave accent | 96 | `60` |
| `{` | Left brace | 123 | `7B` |
| ` | ` | Vertical bar | 124 | `7C` |
| `}` | Right brace | 125 | `7D` |
| `~` | Tilde | 126 | `7E` |

# 🗁 ASCII Decimal, Binary, and Hexadecimal

ASCII values are commonly represented in decimal, binary, or hexadecimal.

Example using the character `A`:

```text
Character: A

Decimal:     65
Binary:      01000001
Hexadecimal: 41
```

These are different representations of the same numerical value.

| Number System | Representation |
| --- | --- |
| Decimal | `65` |
| Binary | `01000001` |
| Hexadecimal | `41` |

Hexadecimal is particularly common when inspecting encoded data and debugging network or file contents.

# 🗁 ASCII Encoding Example

Consider the text:

```text
Hello
```

Each character has an ASCII value.

| Character | Decimal | Hexadecimal | Binary |
| --- | ---: | --- | --- |
| H | 72 | `48` | `01001000` |
| e | 101 | `65` | `01100101` |
| l | 108 | `6C` | `01101100` |
| l | 108 | `6C` | `01101100` |
| o | 111 | `6F` | `01101111` |

The complete hexadecimal representation is:

```text
48 65 6C 6C 6F
```

The binary representation is:

```text
01001000 01100101 01101100 01101100 01101111
```

# 🗁 ASCII Encoding in JavaScript

JavaScript strings use UTF-16 code units internally, but ASCII characters have the same numerical values in UTF-8.

Use `TextEncoder` to convert text into UTF-8 bytes.

```javascript
const text = "Hello";

const encoder = new TextEncoder();

const bytes = encoder.encode(text);

console.log(bytes);
```

Output:

```text
Uint8Array(5) [72, 101, 108, 108, 111]
```

Convert the byte values to hexadecimal:

```javascript
const hex = Array.from(bytes)
  .map(byte => byte.toString(16).padStart(2, "0"))
  .join(" ");

console.log(hex);
```

Output:

```text
48 65 6c 6c 6f
```

# 🗁 ASCII Decoding in JavaScript

Use `TextDecoder` to convert UTF-8 bytes back into text.

```javascript
const bytes = new Uint8Array([
  72, 101, 108, 108, 111
]);

const decoder = new TextDecoder("utf-8");

const text = decoder.decode(bytes);

console.log(text);
```

Output:

```text
Hello
```

Because the example contains only ASCII characters, UTF-8 decoding produces the expected result.

# 🗁 ASCII Encoding in Python

Python provides built-in string encoding methods.

```python
text = "Hello"

encoded = text.encode("ascii")

print(encoded)
```

Output:

```text
b'Hello'
```

Inspect the individual byte values:

```python
for byte in encoded:
    print(byte)
```

Output:

```text
72
101
108
108
111
```

# 🗁 ASCII Decoding in Python

Use the `decode()` method to convert ASCII bytes into a string.

```python
encoded = b'Hello'

decoded = encoded.decode("ascii")

print(decoded)
```

Output:

```text
Hello
```

# 🗁 ASCII Limitations

ASCII was designed primarily for English-language communication.

It cannot directly represent most characters used in other writing systems.

For example:

```text
English:
Hello

Bengali:
হ্যালো

Arabic:
مرحبا

Chinese:
你好
```

Trying to encode Bengali text using ASCII in Python raises an encoding error.

```python
text = "বাংলা"

encoded = text.encode("ascii")
```

Typical result:

```text
UnicodeEncodeError
```

UTF-8 supports these characters and is a better choice for modern multilingual applications.

# 🗁 Understanding Extended ASCII

The term **Extended ASCII** commonly refers to 8-bit character encodings that use values from 128 to 255 in addition to the original ASCII range.

An 8-bit byte provides:

\[
2^8 = 256
\]

possible values.

However, Extended ASCII is not one universal character encoding standard.

Different character sets assign different characters to values above 127.

Examples include:

* Windows-1252.
* ISO-8859-1.
* IBM Code Page 437.

A byte value can represent different characters depending on the selected encoding.

This is one reason UTF-8 became important for consistent international text representation.

# 🗁 ASCII and UTF-8 Compatibility

UTF-8 is backward-compatible with ASCII for the original 128 ASCII characters.

This means:

* ASCII characters use one byte in UTF-8.
* Their byte values remain unchanged.
* Existing ASCII text is valid UTF-8.
* Non-ASCII Unicode characters require multiple bytes in UTF-8.

Example:

```text
Character: A

ASCII:
41

UTF-8:
41
```

Another example:

```text
Character: অ

ASCII:
Not representable

UTF-8:
E0 A6 85
```

# 🗁 Practical Example: Checking ASCII Characters

JavaScript can check whether a character belongs to the ASCII range.

```javascript
function isASCII(text) {
  return [...text].every(char => char.codePointAt(0) <= 127);
}

console.log(isASCII("Hello"));
// true

console.log(isASCII("Hello123"));
// true

console.log(isASCII("বাংলা"));
// false
```

Python example:

```python
def is_ascii(text):
    try:
        text.encode("ascii")
        return True
    except UnicodeEncodeError:
        return False


print(is_ascii("Hello"))
# True

print(is_ascii("বাংলা"))
# False
```

# 🗁 Practical Example: Converting Text to ASCII Decimal Values

JavaScript:

```javascript
const text = "ABC";

const values = [...text].map(char => char.codePointAt(0));

console.log(values);
```

Output:

```text
[65, 66, 67]
```

Python:

```python
text = "ABC"

values = [ord(char) for char in text]

print(values)
```

Output:

```text
[65, 66, 67]
```

For ASCII characters, `ord()` returns the corresponding ASCII numerical value.

# 🗁 Practical Example: Converting ASCII Values to Characters

JavaScript:

```javascript
const values = [65, 66, 67];

const text = String.fromCharCode(...values);

console.log(text);
```

Output:

```text
ABC
```

Python:

```python
values = [65, 66, 67]

text = "".join(chr(value) for value in values)

print(text)
```

Output:

```text
ABC
```

# 🗁 Common ASCII Mistakes

* Assuming ASCII supports every language.
* Confusing ASCII with Unicode.
* Treating Extended ASCII as one universal encoding.
* Assuming every character occupies one byte in every encoding.
* Confusing decimal, binary, and hexadecimal representations.
* Assuming UTF-8 and ASCII produce different bytes for ordinary English ASCII characters.
* Trying to encode non-ASCII characters using ASCII without handling encoding errors.

# 🗁 Best Practices

* Use UTF-8 for modern application development.
* Use ASCII when working with systems or protocols that explicitly require it.
* Understand that original ASCII supports only 128 values.
* Do not assume Extended ASCII means one consistent encoding.
* Use hexadecimal when inspecting byte representations.
* Handle encoding errors when processing external text.
* Prefer explicit encoding declarations when reading and writing text files.
* Remember that UTF-8 preserves the original ASCII byte values.

# 🗁 Summary

ASCII is an early character encoding standard that represents 128 character values using seven bits.

Key concepts:

* ASCII supports values from 0 to 127.
* Printable characters occupy values 32–126.
* Values 0–31 and 127 are control characters.
* ASCII represents English letters, digits, punctuation, and basic symbols.
* Extended ASCII refers to multiple incompatible 8-bit encodings.
* ASCII is backward-compatible with UTF-8 for its original 128 characters.
* ASCII cannot represent most non-English writing systems.

Understanding ASCII provides a foundation for learning Unicode and UTF-8.
