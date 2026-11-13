# HTML Table Headings

HTML table headings define labels that describe the meaning of table rows and columns.

The `<th>` element represents a header cell in an HTML table. It helps users understand what information each column or row represents.

Table headings are important for semantic HTML and accessibility because assistive technologies can use them to communicate relationships between header cells and data cells.

# 🗁 Introduction to Table Headings

A table heading identifies the meaning of a column or row.

Consider an employee table:

| Employee ID | Name | Department |
| --- | --- | --- |
| 101 | John | Development |
| 102 | Sarah | Design |
| 103 | Alex | Marketing |

The first row contains column headings:

* Employee ID
* Name
* Department

These labels help users understand the data displayed below them.

HTML uses the `<th>` element to represent these headings.

# 🗁 The th Element

The `<th>` element stands for **Table Header**.

It defines a header cell inside a table row.

### 🖆 Syntax

```html
<th>Heading</th>
```

### 🖆 Example

```html
<table>
  <tr>
    <th>ID</th>
    <th>Name</th>
    <th>Department</th>
  </tr>

  <tr>
    <td>101</td>
    <td>John</td>
    <td>Development</td>
  </tr>

  <tr>
    <td>102</td>
    <td>Sarah</td>
    <td>Design</td>
  </tr>
</table>
```

The `<th>` elements define the column labels, while `<td>` elements contain ordinary data.

### 🖆 Default Browser Behavior

Browsers typically render `<th>` elements with:

* Bold text.
* Centered text alignment.
* Header-cell semantics.

These are default presentation styles, not requirements. CSS can customize their appearance.

# 🗁 Creating Column Headings

Column headings describe the information contained in each column.

They are commonly placed inside a row within `<thead>`.

```html
<table>
  <thead>
    <tr>
      <th>Product ID</th>
      <th>Product Name</th>
      <th>Price</th>
      <th>Stock</th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>P001</td>
      <td>Laptop</td>
      <td>$900</td>
      <td>15</td>
    </tr>

    <tr>
      <td>P002</td>
      <td>Keyboard</td>
      <td>$50</td>
      <td>40</td>
    </tr>
  </tbody>
</table>
```

The `<thead>` element groups the header row, while `<tbody>` groups the main data rows.

Using these elements makes the table structure easier to understand and maintain.

# 🗁 Creating Row Headings

A table heading can also describe a row rather than a column.

For example, a schedule may use the days of the week as row headings.

```html
<table>
  <tbody>
    <tr>
      <th scope="row">Monday</th>
      <td>HTML</td>
      <td>CSS</td>
    </tr>

    <tr>
      <th scope="row">Tuesday</th>
      <td>JavaScript</td>
      <td>TypeScript</td>
    </tr>

    <tr>
      <th scope="row">Wednesday</th>
      <td>React</td>
      <td>Node.js</td>
    </tr>
  </tbody>
</table>
```

The first cell in each row is a heading describing the remaining cells.

The `scope="row"` attribute identifies each heading as a row header.

# 🗁 The Difference Between th and td

Both `<th>` and `<td>` define table cells, but they have different semantic purposes.

| Feature | `<th>` | `<td>` |
| --- | --- | --- |
| Full name | Table Header | Table Data |
| Purpose | Defines a header cell | Defines a data cell |
| Semantic meaning | Labels related data | Contains ordinary data |
| Default appearance | Usually bold and centered | Usually normal and left-aligned |
| Accessibility | Provides header relationships | Provides cell content |

Example:

```html
<table>
  <tr>
    <th>Name</th>
    <td>John</td>
  </tr>

  <tr>
    <th>Role</th>
    <td>Developer</td>
  </tr>
</table>
```

Here, the headings identify the information represented by each row.

Do not use `<th>` simply to make text bold. Use CSS for visual emphasis when the cell is not actually a table heading.

# 🗁 The scope Attribute

The `scope` attribute identifies whether a header cell applies to a row, column, or group of rows or columns.

It helps assistive technologies understand the relationships between headings and data cells.

### 🖆 scope="col"

Identifies a heading for a column.

```html
<table>
  <thead>
    <tr>
      <th scope="col">Name</th>
      <th scope="col">Role</th>
      <th scope="col">Experience</th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>John</td>
      <td>Developer</td>
      <td>3 years</td>
    </tr>
  </tbody>
</table>
```

### 🖆 scope="row"

Identifies a heading for a row.

```html
<table>
  <tbody>
    <tr>
      <th scope="row">John</th>
      <td>Developer</td>
      <td>3 years</td>
    </tr>

    <tr>
      <th scope="row">Sarah</th>
      <td>Designer</td>
      <td>5 years</td>
    </tr>
  </tbody>
</table>
```

### 🖆 scope="colgroup"

Identifies a heading that applies to a group of columns.

```html
<table>
  <colgroup span="2"></colgroup>
  <colgroup span="2"></colgroup>

  <thead>
    <tr>
      <th colspan="2" scope="colgroup">Personal Information</th>
      <th colspan="2" scope="colgroup">Employment Information</th>
    </tr>

    <tr>
      <th scope="col">Name</th>
      <th scope="col">Email</th>
      <th scope="col">Department</th>
      <th scope="col">Position</th>
    </tr>
  </thead>
</table>
```

### 🖆 scope="rowgroup"

Identifies a heading that applies to a group of rows.

This can be useful when a table contains multiple logical row groups.

```html
<table>
  <tbody>
    <tr>
      <th rowspan="2" scope="rowgroup">Development</th>
      <td>John</td>
    </tr>

    <tr>
      <td>Alex</td>
    </tr>
  </tbody>
</table>
```

### 🖆 Scope Values

| Value | Meaning |
| --- | --- |
| `col` | Applies to a column |
| `row` | Applies to a row |
| `colgroup` | Applies to a group of columns |
| `rowgroup` | Applies to a group of rows |

For straightforward tables, `scope="col"` and `scope="row"` are commonly sufficient.

# 🗁 Associating Headers with Data Cells

For simple tables, browsers and assistive technologies can generally determine header relationships from the table structure and the `scope` attribute.

For complex tables with multiple levels of headings, the `id` and `headers` attributes can explicitly associate data cells with their corresponding header cells.

### 🖆 Using the id Attribute on Headers

```html
<table>
  <thead>
    <tr>
      <th id="product">Product</th>
      <th id="price">Price</th>
      <th id="stock">Stock</th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td headers="product">Laptop</td>
      <td headers="price">$900</td>
      <td headers="stock">15</td>
    </tr>
  </tbody>
</table>
```

The `headers` attribute references the `id` values of associated header cells.

A cell can reference multiple header IDs when necessary.

```html
<td headers="department employee">
  John
</td>
```

Use explicit associations when a complex table's relationships cannot be communicated reliably through simple structural markup.

# 🗁 Multiple and Grouped Headings

Some tables need multiple heading levels.

For example, a financial report may group several columns under one category.

```html
<table>
  <thead>
    <tr>
      <th rowspan="2" scope="col">Department</th>
      <th colspan="2" scope="colgroup">Quarterly Revenue</th>
    </tr>

    <tr>
      <th scope="col">Q1</th>
      <th scope="col">Q2</th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <th scope="row">Development</th>
      <td>$20,000</td>
      <td>$25,000</td>
    </tr>

    <tr>
      <th scope="row">Marketing</th>
      <td>$15,000</td>
      <td>$18,000</td>
    </tr>
  </tbody>
</table>
```

This example uses:

* `rowspan` to make the Department heading cover two header rows.
* `colspan` to group the quarterly revenue columns.
* `scope="colgroup"` to identify the grouped column heading.
* `scope="row"` to identify department row headings.

For complex tables, carefully plan the header structure before writing the markup.

# 🗁 Styling Table Headings with CSS

CSS can customize the appearance of table headings.

```html
<table class="employee-table">
  <thead>
    <tr>
      <th>ID</th>
      <th>Name</th>
      <th>Role</th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>101</td>
      <td>John</td>
      <td>Developer</td>
    </tr>

    <tr>
      <td>102</td>
      <td>Sarah</td>
      <td>Designer</td>
    </tr>
  </tbody>
</table>
```

```css
.employee-table {
  width: 100%;
  border-collapse: collapse;
}

.employee-table th,
.employee-table td {
  padding: 12px;
  border: 1px solid #ddd;
  text-align: left;
}

.employee-table thead th {
  background-color: #333;
  color: white;
  font-weight: 600;
}
```

### 🖆 Styling Specific Headings

```css
.employee-table th:first-child {
  width: 100px;
}

.employee-table th:last-child {
  text-align: center;
}
```

### 🖆 Aligning Numeric Headings

For tables containing numeric values, right alignment can improve readability.

```css
.numeric-heading {
  text-align: right;
}
```

Apply the same alignment to the corresponding numeric data cells.

# 🗁 Sticky Table Headings

For long tables, sticky column headings can help users remember what each column represents while scrolling.

```html
<div class="table-container">
  <table>
    <thead>
      <tr>
        <th>ID</th>
        <th>Name</th>
        <th>Department</th>
      </tr>
    </thead>

    <tbody>
      <tr>
        <td>101</td>
        <td>John</td>
        <td>Development</td>
      </tr>

      <tr>
        <td>102</td>
        <td>Sarah</td>
        <td>Design</td>
      </tr>
    </tbody>
  </table>
</div>
```

```css
.table-container {
  max-height: 350px;
  overflow: auto;
}

table {
  width: 100%;
  border-collapse: separate;
  border-spacing: 0;
}

thead th {
  position: sticky;
  top: 0;
  background-color: #333;
  color: white;
  z-index: 1;
}
```

The `position: sticky` property keeps the header cells visible while scrolling within the container.

A background color is important so that table content does not visually show through the sticky headings.

# 🗁 Accessible Table Headings

Table headings play an important role in accessible data presentation.

Screen readers use table structure and header relationships to help users understand the meaning of individual cells.

### 🖆 Use Meaningful Headings

Avoid vague headings such as:

```html
<th>Value</th>
<th>Information</th>
<th>Data</th>
```

Prefer descriptive headings:

```html
<th>Product Name</th>
<th>Unit Price</th>
<th>Available Quantity</th>
```

### 🖆 Use Appropriate Scope

For a simple table:

```html
<table>
  <thead>
    <tr>
      <th scope="col">Name</th>
      <th scope="col">Department</th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <th scope="row">John</th>
      <td>Development</td>
    </tr>
  </tbody>
</table>
```

### 🖆 Provide a Table Caption

A caption describes the purpose of the table.

```html
<table>
  <caption>Employee Information</caption>

  <thead>
    <tr>
      <th scope="col">Name</th>
      <th scope="col">Role</th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>John</td>
      <td>Developer</td>
    </tr>
  </tbody>
</table>
```

The `<caption>` element is covered in more detail in the table sections chapter.

# 🗁 Best Practices

* Use `<th>` for actual table headings.
* Use `<td>` for ordinary data cells.
* Use `scope="col"` for column headings.
* Use `scope="row"` for row headings.
* Use `scope="colgroup"` and `scope="rowgroup"` when appropriate.
* Use meaningful and descriptive heading text.
* Use `id` and `headers` for complex header relationships.
* Use `<thead>` to group header rows.
* Include a `<caption>` when the table needs a title or description.
* Keep heading structures logical and understandable.
* Ensure heading text has sufficient visual contrast.
* Do not use `<th>` simply to make text bold.

# 🗁 Common Mistakes

| Mistake | Correct approach |
| --- | --- |
| Using `<td>` for column headings | Use `<th>` |
| Using `<th>` only for bold styling | Use CSS for visual emphasis |
| Omitting meaningful heading labels | Write descriptive column and row headings |
| Using incorrect `scope` values | Match scope to the actual header relationship |
| Creating complex tables without clear associations | Use appropriate scope or `headers` relationships |
| Using duplicate IDs | Give every header cell a unique ID |
| Relying only on visual styling | Provide semantic HTML relationships |

# 🗁 Practical Exercises

**Exercise: Employee Table Headings**

Create an employee table with ID, name, department, and salary headings. Use `scope="col"`.

**Exercise: Row Headings**

Create a product comparison table where each product name is a row heading using `scope="row"`.

**Exercise: Grouped Headings**

Build a financial table with grouped quarterly revenue columns.

**Exercise: Explicit Header Associations**

Create a complex table using header IDs and the `headers` attribute.

**Exercise: Sticky Headings**

Build a long table with sticky column headings using CSS.

**Exercise: Accessibility Improvement**

Review an existing table and improve its heading structure, labels, and header relationships.

# 🗁 Interview Questions

**What is the `<th>` element?**

The `<th>` element defines a header cell in an HTML table.

**What is the difference between `<th>` and `<td>`?**

`<th>` represents a heading that labels related data, while `<td>` represents an ordinary data cell.

**What is the scope attribute?**

The `scope` attribute identifies whether a table heading applies to a row, column, or group of rows or columns.

**What is scope="col"?**

It identifies a header cell as a column heading.

**What is scope="row"?**

It identifies a header cell as a row heading.

**What is the purpose of the headers attribute?**

It explicitly associates a data cell with one or more header cells using their IDs.

**Can a table have multiple heading rows?**

Yes. A table can contain multiple header rows, commonly grouped inside `<thead>`.

**Why are table headings important for accessibility?**

They communicate the relationships between table cells and their corresponding headings, helping assistive technologies present tabular information correctly.

**How can you make table headings sticky?**

Use CSS `position: sticky` with an appropriate offset and background styling.

**Should `<th>` be used to make text bold?**

No. Use CSS for visual emphasis. `<th>` should represent actual table header information.

# 🗁 References

* [MDN: HTML `<th>` Element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/th)
* [MDN: HTML `<td>` Element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/td)
* [MDN: HTML `<thead>` Element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/thead)
* [MDN: Table Accessibility](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Structuring_content/Table_accessibility)
* [WHATWG: Tabular Data](https://html.spec.whatwg.org/multipage/tables.html)
