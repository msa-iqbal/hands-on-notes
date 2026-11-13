# Encoding Basics

Encoding is the process of converting information from one representation into another so that computers can store, process, and transmit it.

Computers work with binary data, represented by bits and bytes. However, humans work with characters, symbols, and text.

Character encoding defines how textual characters are represented as numerical values and converted into bytes.

Understanding encoding is essential for:

* Web development and API communication.
* Reading and writing files.
* Database storage.
* Internationalization and localization.
* Data transmission.
* Text processing and manipulation.
* Troubleshooting corrupted or incorrectly displayed text.

# 🗁 Understanding Bits

A bit is the smallest unit of digital information.

A bit can have only two possible values:

* `0`
* `1`

Computers represent information using combinations of these binary values.

Example:

```text
Binary:
01001000
```

This sequence contains eight bits.

# 🗁 Understanding Bytes

A byte consists of eight bits.

```text
1 Byte = 8 Bits
```

Example:

```text
01001000
```

This is one byte of binary data.

A byte can represent 256 different values, ranging from 0 to 255.

```text
Minimum: 00000000 = 0
Maximum: 11111111 = 255
```

Bytes are commonly used to represent encoded text, images, audio, video, and other digital information.

# 🗁 Understanding Characters

A character is a textual unit used to represent information.

Examples:

```text
A
B
7
@
$
€
অ
中
😀
```

Characters are what humans read and write.

However, computers do not directly store abstract characters. They store numerical values represented as binary data.

An encoding system defines how characters are converted into bytes.

# 🗁 Understanding Code Points

A code point is a numerical value assigned to a character in a character set such as Unicode.

Unicode code points are written using the prefix `U+` followed by hexadecimal digits.

Examples:

| Character | Unicode Code Point |
| --- | --- |
| A | U+0041 |
| B | U+0042 |
| a | U+0061 |
| € | U+20AC |
| অ | U+0985 |
| 中 | U+4E2D |
| 😀 | U+1F600 |

A code point identifies a character, but it does not directly specify how that character is stored as bytes.

That is the responsibility of an encoding such as UTF-8 or UTF-16.

# 🗁 Understanding Character Encoding

Character encoding defines how characters or Unicode code points are represented as bytes.

For example, the character `A` has the Unicode code point `U+0041`.

Its UTF-8 representation is:

```text
Character: A
Code Point: U+0041

Decimal: 65
Hexadecimal: 41
Binary: 01000001

UTF-8 Bytes:
41
```

Another example:

```text
Character: €

Code Point: U+20AC

UTF-8 Bytes:
E2 82 AC
```

Different characters can require different numbers of bytes.

# 🗁 Understanding Encoding

Encoding converts text into bytes using a specified character encoding.

```text
Text
  |
  v
Character Encoding
  |
  v
Bytes
```

Example using JavaScript:

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

Each number represents one UTF-8 byte.

# 🗁 Understanding Decoding

Decoding converts encoded bytes back into readable text.

```text
Bytes
  |
  v
Character Decoding
  |
  v
Text
```

JavaScript example:

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

The decoder must use the correct encoding to interpret the bytes properly.

# 🗁 Encoding and Decoding in Python

Python provides built-in methods for encoding and decoding strings.

Encoding:

```python
text = "Hello"

encoded = text.encode("utf-8")

print(encoded)
```

Output:

```text
b'Hello'
```

Decoding:

```python
encoded = b'Hello'

decoded = encoded.decode("utf-8")

print(decoded)
```

Output:

```text
Hello
```

Python strings represent Unicode text, while `bytes` objects represent sequences of byte values.

# 🗁 Understanding ASCII and Unicode

ASCII and Unicode are character encoding-related standards with different purposes.

**ASCII**

ASCII defines 128 character values using seven bits.

It primarily represents:

* English letters.
* Decimal digits.
* Punctuation.
* Control characters.

**Unicode**

Unicode defines a universal character set that supports characters from many writing systems.

It includes:

* Latin characters.
* Bengali characters.
* Arabic characters.
* Chinese characters.
* Japanese characters.
* Mathematical symbols.
* Emoji.

Unicode code points can be encoded using UTF-8, UTF-16, or UTF-32.

# 🗁 Character Set vs Character Encoding

These terms are related but not identical.

**Character Set**

A character set defines a collection of characters and their associated identifiers.

For example, Unicode defines code points for characters.

**Character Encoding**

An encoding defines how character values are represented as bytes.

Examples:

| Character Set or Standard | Encoding |
| --- | --- |
| ASCII | ASCII encoding |
| Unicode | UTF-8 |
| Unicode | UTF-16 |
| Unicode | UTF-32 |

Unicode is not itself a specific byte encoding. UTF-8, UTF-16, and UTF-32 are Unicode encoding forms.

# 🗁 Understanding Bytes vs Characters

A character does not always occupy one byte.

The number of bytes depends on the encoding.

Example using UTF-8:

| Character | UTF-8 Bytes | Byte Length |
| --- | --- | ---: |
| A | `41` | 1 |
| B | `42` | 1 |
| € | `E2 82 AC` | 3 |
| অ | `E0 A6 85` | 3 |
| 😀 | `F0 9F 98 80` | 4 |

This distinction is important when calculating file sizes, network payload sizes, and string lengths.

# 🗁 String Length vs Byte Length in JavaScript

JavaScript provides different ways to measure strings and their encoded byte representations.

Example:

```javascript
const text = "Hello";

console.log(text.length);
// 5

const encoder = new TextEncoder();

const bytes = encoder.encode(text);

console.log(bytes.length);
// 5
```

For ASCII text, these values happen to be equal.

However, consider an emoji:

```javascript
const text = "😀";

console.log(text.length);
// 2

const bytes = new TextEncoder().encode(text);

console.log(bytes.length);
// 4
```

JavaScript's `length` property counts UTF-16 code units, not UTF-8 bytes or user-perceived characters.

# 🗁 String Length vs Byte Length in Python

Python provides separate ways to measure character and byte lengths.

Example:

```python
text = "Hello"

print(len(text))
# 5

print(len(text.encode("utf-8")))
# 5
```

For a Bengali character:

```python
text = "অ"

print(len(text))
# 1

print(len(text.encode("utf-8")))
# 3
```

The string contains one Python code point, but its UTF-8 representation requires three bytes.

For complex text, the number of Unicode code points may also differ from the number of user-perceived characters.

# 🗁 Encoding vs Serialization

Encoding and serialization are different operations.

**Encoding**

Converts characters into bytes.

Examples:

* UTF-8
* UTF-16
* ASCII

**Serialization**

Converts structured data into a format suitable for storage or transmission.

Examples:

* JSON
* YAML
* XML
* Protocol Buffers

Consider this JSON object:

```json
{
  "name": "Alice",
  "age": 25
}
```

JSON serialization produces a textual representation of the object.

UTF-8 encoding converts that textual representation into bytes.

```text
JavaScript Object
       |
       v
JSON Serialization
       |
       v
JSON Text
       |
       v
UTF-8 Encoding
       |
       v
Bytes
```

# 🗁 Encoding vs Encryption

Encoding and encryption serve different purposes.

| Feature | Encoding | Encryption |
| --- | --- | --- |
| Purpose | Change data representation | Protect data confidentiality |
| Secret key | Not required | Usually required |
| Reversible | Yes | Yes, with the correct key |
| Security | Does not provide confidentiality | Provides confidentiality when properly implemented |
| Example | Base64, UTF-8 | AES, RSA |

Example:

```text
Original:
Hello

Base64:
SGVsbG8=
```

Base64 is an encoding technique. It does not protect the original text from being read.

# 🗁 Understanding Mojibake

Mojibake occurs when text is decoded using an incorrect character encoding.

Example:

```text
Original:
বাংলা

Incorrect display:
à¦¬à¦¾à¦‚à¦²à¦¾
```

This often happens when:

* A file is saved using one encoding but opened using another.
* An HTTP response declares an incorrect charset.
* Database and application encodings are incompatible.
* Text is converted multiple times incorrectly.

To prevent mojibake:

* Prefer UTF-8 for modern applications.
* Declare character encoding explicitly when necessary.
* Use consistent database and application encodings.
* Decode bytes using the encoding used to encode them.

# 🗁 Encoding in HTTP Communication

Web applications exchange text through HTTP requests and responses.

The `Content-Type` header identifies the media type and can specify a character encoding.

Example:

```http
Content-Type: text/html; charset=utf-8
```

For JSON:

```http
Content-Type: application/json; charset=utf-8
```

UTF-8 is the interoperable encoding expected for JSON exchanged between systems.

For HTML documents, declare UTF-8:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Encoding Example</title>
</head>
<body>
  <h1>Hello World!</h1>
</body>
</html>
```

# 🗁 Encoding in File Handling

Text files contain bytes. Applications need to interpret those bytes using the appropriate encoding.

JavaScript example:

```javascript
import fs from "node:fs";

const text = "Hello বাংলা";

fs.writeFileSync("example.txt", text, "utf8");

const content = fs.readFileSync("example.txt", "utf8");

console.log(content);
```

Python example:

```python
text = "Hello বাংলা"

with open("example.txt", "w", encoding="utf-8") as file:
    file.write(text)

with open("example.txt", "r", encoding="utf-8") as file:
    content = file.read()

print(content)
```

Explicitly specifying UTF-8 helps applications handle multilingual text consistently.

# 🗁 Common Encoding Mistakes

**Assuming one character equals one byte**

Incorrect assumption:

```text
Every character = 1 byte
```

Correct understanding:

```text
Character size depends on the encoding.
```

**Confusing Unicode with UTF-8**

Unicode defines code points. UTF-8 defines how those code points are encoded into bytes.

**Using the wrong decoder**

Bytes encoded using UTF-8 should be decoded using UTF-8.

**Treating Base64 as encryption**

Base64 changes representation but does not provide confidentiality.

**Ignoring file encoding**

A text file saved in one encoding may be interpreted incorrectly by an application expecting another.

**Confusing string length with byte length**

String length and encoded byte length are different measurements.

# 🗁 Best Practices

* Prefer UTF-8 for modern web applications and text files.
* Specify encoding explicitly when reading and writing files.
* Understand the difference between characters, code points, and bytes.
* Use the correct decoder for incoming byte data.
* Avoid unnecessary encoding and decoding operations.
* Validate external text data when processing it.
* Keep database and application encoding configurations compatible.
* Understand the difference between character encoding and serialization.
* Never treat encoding as a security mechanism.
* Test multilingual text in applications that support international users.

# 🗁 Summary

Encoding defines how textual information is represented as bytes.

Important concepts:

* A bit is a binary digit.
* A byte consists of eight bits.
* A character represents textual information.
* A Unicode code point identifies a character.
* Character encoding converts character values into bytes.
* Decoding converts bytes back into text.
* UTF-8 is a widely used Unicode encoding.
* String length and byte length are not always equal.
* Encoding is different from serialization, encryption, and hashing.
* Incorrect encoding interpretation can cause mojibake.

Understanding these fundamentals provides the foundation for working with ASCII, Unicode, UTF-8, Base64, and URL encoding.
