# Unicode

Unicode is a universal character encoding standard that defines a unique identifier for characters used by writing systems around the world.

It provides a consistent way to represent text across different programming languages, operating systems, applications, databases, and communication protocols.

Unicode supports far more than English letters and numbers. It includes:

* Characters from writing systems such as Latin, Arabic, Bengali, Chinese, and Japanese.
* Mathematical and scientific symbols.
* Punctuation marks.
* Currency symbols.
* Combining marks.
* Emoji and other symbols.
* Special formatting and control characters.

# 🗁 What Is Unicode?

Unicode is a standard that assigns a unique numerical value to each defined character or abstract text element.

These numerical values are called **code points**.

For example:

| Character | Description | Unicode Code Point |
| --- | --- | --- |
| `A` | Latin capital letter A | `U+0041` |
| `a` | Latin small letter a | `U+0061` |
| `0` | Digit zero | `U+0030` |
| `€` | Euro sign | `U+20AC` |
| `অ` | Bengali letter A | `U+0985` |
| `中` | Chinese character | `U+4E2D` |
| `😀` | Grinning face emoji | `U+1F600` |

Unicode gives each code point a unique identity, independent of how it is stored in memory or transmitted over a network.

# 🗁 Unicode Is Not an Encoding

Unicode and UTF-8 are related, but they are not the same thing.

* **Unicode** defines characters and their code points.
* **UTF-8** defines how Unicode code points are represented as bytes.
* **UTF-16** represents Unicode code points using one or two 16-bit code units.
* **UTF-32** represents each Unicode code point using a 32-bit code unit.

For example, the character `A` has the Unicode code point `U+0041`.

Its representations are:

| Representation | Value |
| --- | --- |
| Unicode code point | `U+0041` |
| UTF-8 | `41` |
| UTF-16 | `0041` |
| UTF-32 | `00000041` |

The character remains the same. Only its encoding representation changes.

# 🗁 Unicode Code Points

A Unicode code point is a numerical value assigned to a character or other Unicode-defined element.

Code points are conventionally written using the prefix `U+`, followed by four to six hexadecimal digits.

Examples:

```text
A       U+0041
B       U+0042
€       U+20AC
অ       U+0985
😀      U+1F600
```

Unicode defines the code point range:

```text
U+0000 to U+10FFFF
```

This provides a total of 1,114,112 possible code point positions.

Not every position represents an assigned character.

Some code points are reserved, unassigned, or used for special purposes.

# 🗁 Unicode Scalar Values

A Unicode scalar value is any Unicode code point except the surrogate range.

The surrogate range is:

```text
U+D800 to U+DFFF
```

Therefore:

* Valid scalar values: `U+0000` through `U+D7FF`.
* Invalid scalar values: `U+D800` through `U+DFFF`.
* Valid scalar values: `U+E000` through `U+10FFFF`.

Surrogate code points exist to support UTF-16 encoding. They are not independent Unicode characters and cannot be encoded as individual Unicode scalar values in UTF-8.

This distinction is important when implementing Unicode processing and encoding validation.

# 🗁 Unicode Planes

Unicode divides its code point space into 17 planes.

Each plane contains 65,536 code point positions.

The planes are identified from Plane 0 through Plane 16.

| Plane | Range | Description |
| --- | --- | --- |
| Plane 0 | `U+0000–U+FFFF` | Basic Multilingual Plane (BMP) |
| Plane 1 | `U+10000–U+1FFFF` | Supplementary Multilingual Plane |
| Plane 2 | `U+20000–U+2FFFF` | Supplementary Ideographic Plane |
| Plane 3 | `U+30000–U+3FFFF` | Tertiary Ideographic Plane |
| Planes 4–13 | `U+40000–U+DFFFF` | Currently mostly reserved |
| Plane 14 | `U+E0000–U+EFFFF` | Supplementary Special-purpose Plane |
| Plane 15 | `U+F0000–U+FFFFF` | Supplementary Private Use Area-A |
| Plane 16 | `U+100000–U+10FFFF` | Supplementary Private Use Area-B |

# 🗁 Basic Multilingual Plane (BMP)

The Basic Multilingual Plane is Plane 0.

Its range is:

```text
U+0000 to U+FFFF
```

It contains many commonly used characters, including:

* Latin characters.
* Bengali characters.
* Arabic characters.
* Chinese and Japanese characters.
* Common punctuation.
* Mathematical symbols.
* Most frequently used emoji are outside this plane.

Examples:

```text
A       U+0041
অ       U+0985
中      U+4E2D
```

Most commonly used characters in everyday text belong to the BMP.

# 🗁 Supplementary Planes

Supplementary planes contain Unicode code points outside the BMP.

Their range is:

```text
U+10000 to U+10FFFF
```

They include:

* Additional historical writing systems.
* Extended ideographic characters.
* Musical notation.
* Specialized mathematical symbols.
* Many emoji.

For example:

```text
😀      U+1F600
🚀      U+1F680
```

Characters in supplementary planes require special handling in UTF-16 because they are represented using surrogate pairs.

# 🗁 UTF-16 Surrogate Pairs

UTF-16 uses 16-bit code units.

Characters in the BMP generally require one 16-bit code unit, while supplementary characters require two code units.

These two code units form a **surrogate pair**:

* High surrogate: `U+D800` to `U+DBFF`.
* Low surrogate: `U+DC00` to `U+DFFF`.

For example, the emoji `😀` has the code point:

```text
U+1F600
```

Its UTF-16 representation is:

```text
D83D DE00
```

The two values are not two separate characters. Together, they represent one Unicode code point.

This distinction matters when working with JavaScript strings, which use UTF-16 code units.

# 🗁 Characters, Code Points, and Glyphs

These terms describe different concepts.

**Character**

A character is an abstract unit of text, such as a letter, digit, punctuation mark, or symbol.

**Code point**

A code point is the numerical value assigned to a Unicode element.

**Glyph**

A glyph is the visual shape used to display a character or group of characters.

For example:

```text
Character: A
Code point: U+0041
Glyph: The visible shape A
```

A single character can have different glyph appearances depending on:

* Font family.
* Font weight.
* Writing system.
* Text direction.
* Rendering engine.
* Typographic context.

Likewise, multiple code points can combine to produce one visible glyph.

# 🗁 Grapheme Clusters

A grapheme cluster is a sequence of one or more Unicode code points that users generally perceive as one character.

This is important because one visible character does not always correspond to one code point.

Consider an accented character:

```text
é
```

It can be represented in two ways.

Precomposed form:

```text
U+00E9
```

Decomposed form:

```text
U+0065 U+0301
```

The decomposed form consists of:

* `e` — Latin small letter e.
* `◌́` — Combining acute accent.

Both forms can appear visually identical, but they contain different code point sequences.

Emoji can also consist of multiple code points.

For example:

```text
👨‍👩‍👧‍👦
```

This family emoji uses multiple emoji code points joined using Zero Width Joiner (ZWJ) characters.

# 🗁 Combining Marks

Combining marks are Unicode characters that modify the appearance of a preceding character.

They are commonly used for accents, diacritics, and other writing-system features.

Examples:

```text
e + ◌́ = é
a + ◌̈ = ä
n + ◌̃ = ñ
```

The combining marks do not necessarily occupy an independent visual position.

Their rendering depends on the font and text-rendering system.

When processing text, do not assume that one code point equals one visible character.

# 🗁 Unicode Normalization

Unicode normalization provides standardized ways to represent text that may look identical but contain different code point sequences.

Normalization is especially important for:

* String comparison.
* Search functionality.
* Database storage.
* User input processing.
* File names.
* Identifiers.

Unicode defines four major normalization forms.

| Form | Full Name | Description |
| --- | --- | --- |
| NFC | Canonical Composition | Combines compatible canonical sequences where possible |
| NFD | Canonical Decomposition | Decomposes characters into canonical sequences |
| NFKC | Compatibility Composition | Applies compatibility decomposition and composition |
| NFKD | Compatibility Decomposition | Applies compatibility decomposition |

# 🗁 Canonical Equivalence

Canonical equivalence means two Unicode sequences represent the same abstract text even though their code point sequences differ.

For example:

```text
é
```

Precomposed:

```text
U+00E9
```

Decomposed:

```text
U+0065 U+0301
```

These sequences are canonically equivalent.

NFC generally converts them into a composed representation when one is available.

NFD generally converts them into decomposed representations.

# 🗁 Compatibility Equivalence

Compatibility equivalence covers characters that may have different appearances or semantic distinctions but are treated as equivalent under compatibility normalization.

For example, the full-width Latin letter:

```text
Ａ
```

Compatibility-normalizes to:

```text
A
```

Other examples include certain presentation forms, ligatures, and special numeral characters.

NFKC and NFKD apply compatibility mappings.

Compatibility normalization can change distinctions that matter in some applications, so it should not be applied indiscriminately.

# 🗁 Unicode Normalization in JavaScript

JavaScript provides the `normalize()` string method.

Example:

```javascript
const composed = "\u00E9";
const decomposed = "\u0065\u0301";

console.log(composed === decomposed);
// false

console.log(composed.normalize("NFC") === decomposed.normalize("NFC"));
// true

console.log(composed.normalize("NFD") === decomposed.normalize("NFD"));
// true
```

You can also use compatibility normalization:

```javascript
const fullWidth = "Ａ";

console.log(fullWidth.normalize("NFKC"));
// A
```

Supported forms:

```javascript
text.normalize("NFC");
text.normalize("NFD");
text.normalize("NFKC");
text.normalize("NFKD");
```

# 🗁 Unicode Normalization in Python

Python provides the `unicodedata` module.

Example:

```python
import unicodedata

composed = "\u00E9"
decomposed = "\u0065\u0301"

print(composed == decomposed)
# False

normalized_composed = unicodedata.normalize("NFC", composed)
normalized_decomposed = unicodedata.normalize("NFC", decomposed)

print(normalized_composed == normalized_decomposed)
# True
```

Compatibility normalization:

```python
full_width = "Ａ"

print(unicodedata.normalize("NFKC", full_width))
# A
```

# 🗁 Unicode Blocks and Scripts

Unicode organizes code points into blocks and assigns characters to scripts.

**Unicode blocks** describe ranges of code points grouped for organizational purposes.

Examples:

| Block | Range |
| --- | --- |
| Basic Latin | `U+0000–U+007F` |
| Latin-1 Supplement | `U+0080–U+00FF` |
| Greek and Coptic | `U+0370–U+03FF` |
| Bengali | `U+0980–U+09FF` |
| Arabic | `U+0600–U+06FF` |
| CJK Unified Ideographs | `U+4E00–U+9FFF` |

**Scripts** describe writing systems used by languages.

Examples:

* Latin.
* Bengali.
* Arabic.
* Cyrillic.
* Devanagari.
* Han.

A script can contain characters distributed across multiple Unicode blocks.

A block and a script are therefore not interchangeable concepts.

# 🗁 Unicode Versions

Unicode is maintained and updated over time.

New versions can introduce:

* Additional characters.
* New emoji.
* New writing-system support.
* Additional symbols.
* Changes to character properties.
* Improvements to normalization and related specifications.

Software that relies on Unicode properties may behave differently depending on the Unicode version supported by its runtime or libraries.

For official character information and version history, consult:

* Unicode website: <https://www.unicode.org/>
* Unicode character database: <https://www.unicode.org/Public/UCD/latest/>
* Unicode code charts: <https://www.unicode.org/charts/>

# 🗁 Unicode in JavaScript

JavaScript strings are represented using UTF-16 code units.

This means that the `length` property does not necessarily return the number of Unicode code points or visible characters.

Example:

```javascript
const text = "A😀B";

console.log(text.length);
// 4
```

The result is `4` because:

* `A` uses one UTF-16 code unit.
* `😀` uses two UTF-16 code units.
* `B` uses one UTF-16 code unit.

To iterate over Unicode code points:

```javascript
const text = "A😀B";

for (const character of text) {
    console.log(character);
}
```

Output:

```text
A
😀
B
```

Use `codePointAt()` to retrieve a code point:

```javascript
const emoji = "😀";

console.log(emoji.codePointAt(0).toString(16));
// 1f600
```

Use `String.fromCodePoint()` to create a character from a code point:

```javascript
const character = String.fromCodePoint(0x1F600);

console.log(character);
// 😀
```

# 🗁 Unicode in Python

Python strings represent Unicode text.

Python's built-in functions provide direct access to code points.

Use `ord()` to get a character's numerical value:

```python
character = "😀"

print(ord(character))
# 128512
```

Use `chr()` to create a character from a code point:

```python
character = chr(128512)

print(character)
# 😀
```

Convert the result to hexadecimal:

```python
print(hex(ord("😀")))
# 0x1f600
```

Python also provides Unicode character metadata through `unicodedata`:

```python
import unicodedata

character = "A"

print(unicodedata.name(character))
# LATIN CAPITAL LETTER A

print(unicodedata.category(character))
# Lu
```

# 🗁 String Length and Unicode Pitfalls

String length can mean different things depending on the programming language and operation.

Consider:

```text
A😀B
```

| Measurement | Result |
| --- | ---: |
| JavaScript UTF-16 code units | 4 |
| Unicode code points | 3 |
| Python string length | 3 |
| User-perceived grapheme clusters | 3 |

Now consider:

```text
é
```

This may contain two code points but only one grapheme cluster.

Therefore, there are three important measurements:

* Code units: Encoding-level units used by the programming language.
* Code points: Unicode numerical values.
* Grapheme clusters: User-perceived characters.

Never assume that a string's length directly represents the number of visible characters.

# 🗁 Grapheme Segmentation in JavaScript

JavaScript provides `Intl.Segmenter` for grapheme-aware text segmentation.

Example:

```javascript
const text = "e\u0301😀";

const segmenter = new Intl.Segmenter("en", {
    granularity: "grapheme"
});

const characters = [...segmenter.segment(text)];

console.log(characters.length);
// 2

console.log(characters.map(item => item.segment));
// ["é", "😀"]
```

This is useful when implementing:

* Text length limits.
* Cursor movement.
* Text selection.
* Character counters.
* User-facing text processing.

# 🗁 Grapheme Segmentation in Python

Python's standard string length counts Unicode code points, not grapheme clusters.

For grapheme-aware processing, the third-party `regex` package supports the `\X` pattern.

Install:

```bash
pip install regex
```

Example:

```python
import regex

text = "e\u0301😀"

characters = regex.findall(r"\X", text)

print(characters)
# ['é', '😀']

print(len(characters))
# 2
```

# 🗁 Unicode Security Considerations

Unicode supports many writing systems and special formatting characters. This flexibility can create security problems when text is processed without proper validation.

**Confusable characters**

Some characters from different scripts look nearly identical.

For example:

```text
Latin a:     U+0061
Cyrillic а:  U+0430
```

They may look the same but are different characters.

This can be abused in:

* Domain names.
* Usernames.
* Source code.
* Authentication systems.
* Phishing attacks.

**Invisible characters**

Some Unicode characters are invisible or have no obvious visual representation.

Examples include:

* Zero Width Space (ZWSP).
* Zero Width Joiner (ZWJ).
* Zero Width Non-Joiner (ZWNJ).
* Other formatting characters.

They can cause unexpected string comparisons or display behavior.

**Bidirectional control characters**

Unicode includes characters that influence text direction.

These are important for languages such as Arabic and Hebrew, but can also be abused to make displayed text appear different from its underlying character order.

**Normalization issues**

Applications that compare identifiers without a consistent normalization strategy may treat visually equivalent strings as different values.

# 🗁 Best Practices

* Use UTF-8 for most text storage and data exchange.
* Understand the difference between Unicode code points and UTF encodings.
* Normalize text consistently when appropriate.
* Use NFC as a common default for canonical normalization.
* Use compatibility normalization only when its transformations are appropriate for the application.
* Do not assume one code point equals one visible character.
* Use grapheme-aware segmentation for user-facing character counts.
* Validate identifiers and usernames according to application requirements.
* Be aware of confusable characters and invisible formatting characters.
* Avoid removing Unicode formatting characters indiscriminately because some are necessary for correct language rendering.
* Keep Unicode library and runtime versions up to date.
* Test applications with multiple writing systems, combining marks, and supplementary characters.

# 🗁 Practical Examples

**Example: Inspect a Unicode code point in JavaScript**

```javascript
const character = "অ";

console.log(character.codePointAt(0).toString(16));
// 985

console.log(character.normalize("NFC"));
```

**Example: Inspect Unicode information in Python**

```python
import unicodedata

text = "অ"

for character in text:
    print("Character:", character)
    print("Code point:", hex(ord(character)))
    print("Name:", unicodedata.name(character))
    print("Category:", unicodedata.category(character))
```

**Example: Compare normalized strings**

JavaScript:

```javascript
const first = "\u00E9";
const second = "\u0065\u0301";

console.log(first === second);
// false

console.log(first.normalize("NFC") === second.normalize("NFC"));
// true
```

Python:

```python
import unicodedata

first = "\u00E9"
second = "\u0065\u0301"

print(first == second)
# False

print(
    unicodedata.normalize("NFC", first)
    == unicodedata.normalize("NFC", second)
)
# True
```

**Example: Count code points and grapheme clusters**

JavaScript:

```javascript
const text = "e\u0301😀";

console.log([...text].length);
// 3 code points

const segmenter = new Intl.Segmenter("en", {
    granularity: "grapheme"
});

console.log([...segmenter.segment(text)].length);
// 2 grapheme clusters
```

Python:

```python
import regex

text = "e\u0301😀"

print(len(text))
# 3 code points

print(len(regex.findall(r"\X", text)))
# 2 grapheme clusters
```

# 🗁 Summary

Unicode provides a universal standard for representing text from different languages and writing systems.

Its key concepts include:

* Code points identify Unicode elements.
* Unicode scalar values exclude surrogate code points.
* The BMP contains code points from `U+0000` through `U+FFFF`.
* Supplementary planes contain code points above `U+FFFF`.
* UTF-16 uses surrogate pairs for supplementary code points.
* Grapheme clusters represent user-perceived characters.
* Normalization provides consistent representations of equivalent text.
* Unicode security requires attention to confusable characters, invisible characters, and bidirectional controls.
* UTF-8, UTF-16, and UTF-32 are different ways of encoding Unicode data.

Understanding Unicode is essential when developing applications that process multilingual text, user input, internationalized data, and modern web content.
