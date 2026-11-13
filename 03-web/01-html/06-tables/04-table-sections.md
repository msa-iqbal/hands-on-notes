# HTML Table Sections

HTML table sections organize table rows into meaningful structural groups.

HTML provides three primary elements for grouping rows:

* `<thead>` — groups table header rows.
* `<tbody>` — groups the main table data rows.
* `<tfoot>` — groups table footer or summary rows.

These elements improve the organization, maintainability, and accessibility of HTML tables.

They also make it easier to apply CSS styles to specific sections and work with table data using JavaScript.

# 🗁 Introduction to Table Sections

A table can contain many rows, making its structure difficult to understand if everything is placed directly inside `<table>`.

HTML provides section elements to group related rows.

Consider a sales report:

| Product | Quantity | Price |
| --- | ---: | ---: |
| Laptop | 2 | $1,800 |
| Keyboard | 5 | $250 |
| Mouse | 10 | $150 |
| **Total** | **17** | **$2,200** |

This table can be divided into:

* Header section containing column labels.
* Body section containing product records.
* Footer section containing summary information.

### 🖆 Table Section Structure

```text
<table>
│
├── <thead>
│   └── Header rows
│
├── <tbody>
│   ├── Data row
│   ├── Data row
│   └── Data row
│
└── <tfoot>
    └── Summary row
```

# 🗁 The thead Element

The `<thead>` element groups rows containing table headings.

It is commonly used for column labels and other header information.

### 🖆 Syntax

```html
<thead>
  <tr>
    <th>Heading 1</th>
    <th>Heading 2</th>
  </tr>
</thead>
```

### 🖆 Example

```html
<table>
  <thead>
    <tr>
      <th scope="col">ID</th>
      <th scope="col">Name</th>
      <th scope="col">Role</th>
    </tr>
  </thead>
</table>
```

### 🖆 Important Characteristics

* A table can have at most one `<thead>` element.
* It groups one or more header rows.
* It must appear before the table's `<tbody>` and `<tfoot>` elements in the source structure.
* It can contain multiple `<tr>` elements.
* It provides semantic structure rather than visual styling by itself.

# 🗁 The tbody Element

The `<tbody>` element groups the main data rows of a table.

Most table records are placed inside this element.

### 🖆 Syntax

```html
<tbody>
  <tr>
    <td>Data 1</td>
    <td>Data 2</td>
  </tr>
</tbody>
```

### 🖆 Example

```html
<table>
  <thead>
    <tr>
      <th scope="col">ID</th>
      <th scope="col">Name</th>
      <th scope="col">Department</th>
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

    <tr>
      <td>103</td>
      <td>Alex</td>
      <td>Marketing</td>
    </tr>
  </tbody>
</table>
```

### 🖆 Important Characteristics

* A table can contain multiple `<tbody>` elements.
* Each `<tbody>` groups a logical set of rows.
* It is commonly used for the main table data.
* CSS and JavaScript can target individual table body sections.

# 🗁 The tfoot Element

The `<tfoot>` element groups rows containing summary information for a table.

It is commonly used for totals, calculations, and other footer information.

### 🖆 Syntax

```html
<tfoot>
  <tr>
    <td>Summary</td>
    <td>Total</td>
  </tr>
</tfoot>
```

### 🖆 Example

```html
<table>
  <thead>
    <tr>
      <th scope="col">Product</th>
      <th scope="col">Quantity</th>
      <th scope="col">Price</th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>Laptop</td>
      <td>2</td>
      <td>$1,800</td>
    </tr>

    <tr>
      <td>Keyboard</td>
      <td>5</td>
      <td>$250</td>
    </tr>
  </tbody>

  <tfoot>
    <tr>
      <th scope="row" colspan="2">Total</th>
      <td>$2,050</td>
    </tr>
  </tfoot>
</table>
```

### 🖆 Important Characteristics

* A table can have at most one `<tfoot>` element.
* It groups footer or summary rows.
* It is commonly used for totals and aggregate information.
* It should represent meaningful summary content rather than ordinary table data.

# 🗁 Complete Table Structure

The following example demonstrates how all three section elements work together.

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">

  <title>Sales Report</title>
</head>
<body>

  <h1>Monthly Sales Report</h1>

  <table>
    <caption>Product Sales Summary</caption>

    <thead>
      <tr>
        <th scope="col">Product</th>
        <th scope="col">Quantity</th>
        <th scope="col">Price</th>
      </tr>
    </thead>

    <tbody>
      <tr>
        <td>Laptop</td>
        <td>2</td>
        <td>$1,800</td>
      </tr>

      <tr>
        <td>Keyboard</td>
        <td>5</td>
        <td>$250</td>
      </tr>

      <tr>
        <td>Mouse</td>
        <td>10</td>
        <td>$150</td>
      </tr>
    </tbody>

    <tfoot>
      <tr>
        <th scope="row" colspan="2">Total</th>
        <td>$2,200</td>
      </tr>
    </tfoot>
  </table>

</body>
</html>
```

### 🖆 Understanding the Structure

| Section | Content |
| --- | --- |
| `<caption>` | Describes the table |
| `<thead>` | Contains column headings |
| `<tbody>` | Contains product records |
| `<tfoot>` | Contains the summary row |

The `<caption>` element provides a title or description and is not itself a row group.

# 🗁 Multiple tbody Elements

A table can contain multiple `<tbody>` elements.

This is useful when records are divided into separate logical groups.

Example: Employees grouped by department.

```html
<table>
  <thead>
    <tr>
      <th scope="col">Name</th>
      <th scope="col">Position</th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <th scope="rowgroup" colspan="2">Development</th>
    </tr>

    <tr>
      <td>John</td>
      <td>Frontend Developer</td>
    </tr>

    <tr>
      <td>Alex</td>
      <td>Backend Developer</td>
    </tr>
  </tbody>

  <tbody>
    <tr>
      <th scope="rowgroup" colspan="2">Design</th>
    </tr>

    <tr>
      <td>Sarah</td>
      <td>UI Designer</td>
    </tr>

    <tr>
      <td>Emma</td>
      <td>UX Designer</td>
    </tr>
  </tbody>
</table>
```

Each `<tbody>` represents a separate logical group.

For complex grouped tables, ensure that header relationships are clear and accessible. A simple heading row can also be implemented using a dedicated row header structure appropriate to the table's data.

# 🗁 Table Section Ordering

In modern HTML, a table's structural sections generally follow this order:

```html
<table>
  <caption>Employee Report</caption>

  <thead>
    <!-- Header rows -->
  </thead>

  <tbody>
    <!-- Main data rows -->
  </tbody>

  <tfoot>
    <!-- Footer rows -->
  </tfoot>
</table>
```

The `<tfoot>` element appears after the body sections in modern HTML authoring.

HTML parsing has rules that allow some table structures to be interpreted even when elements are omitted or written in a different permitted arrangement. However, writing a clear and consistent structure is easier to maintain.

### 🖆 Are Table Section Elements Required?

No. A basic table can contain rows without explicitly writing `<thead>`, `<tbody>`, or `<tfoot>`.

Example:

```html
<table>
  <tr>
    <th>Name</th>
    <th>Role</th>
  </tr>

  <tr>
    <td>John</td>
    <td>Developer</td>
  </tr>
</table>
```

Browsers may insert an implicit `<tbody>` when parsing table markup containing direct `<tr>` children.

For predictable styling and scripting, explicitly include the appropriate section elements.

# 🗁 Styling Table Sections with CSS

CSS allows developers to style each table section independently.

### 🖆 Styling the Header

```css
thead {
  background-color: #333;
  color: white;
}
```

### 🖆 Styling the Body

```css
tbody {
  background-color: #f9f9f9;
}
```

### 🖆 Styling the Footer

```css
tfoot {
  background-color: #e8e8e8;
  font-weight: bold;
}
```

### 🖆 Complete Styling Example

```html
<table class="sales-table">
  <thead>
    <tr>
      <th>Product</th>
      <th>Quantity</th>
      <th>Price</th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>Laptop</td>
      <td>2</td>
      <td>$1,800</td>
    </tr>

    <tr>
      <td>Keyboard</td>
      <td>5</td>
      <td>$250</td>
    </tr>
  </tbody>

  <tfoot>
    <tr>
      <th colspan="2">Total</th>
      <td>$2,050</td>
    </tr>
  </tfoot>
</table>
```

```css
.sales-table {
  width: 100%;
  border-collapse: collapse;
}

.sales-table th,
.sales-table td {
  padding: 12px;
  border: 1px solid #ddd;
  text-align: left;
}

.sales-table thead {
  background-color: #333;
  color: white;
}

.sales-table tbody tr:nth-child(even) {
  background-color: #f5f5f5;
}

.sales-table tfoot {
  background-color: #e8e8e8;
  font-weight: bold;
}
```

# 🗁 Working with Table Sections in JavaScript

JavaScript provides properties for accessing table sections.

| Property | Purpose |
| --- | --- |
| `table.tHead` | Returns the table's `<thead>` element |
| `table.tBodies` | Returns a collection of `<tbody>` elements |
| `table.tFoot` | Returns the table's `<tfoot>` element |
| `table.rows` | Returns the table's rows |

### 🖆 Accessing Table Sections

```html
<table id="salesTable">
  <thead>
    <tr>
      <th>Product</th>
      <th>Price</th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>Laptop</td>
      <td>$900</td>
    </tr>
  </tbody>

  <tfoot>
    <tr>
      <td>Total</td>
      <td>$900</td>
    </tr>
  </tfoot>
</table>

<script>
  const table = document.querySelector("#salesTable");

  console.log(table.tHead);
  console.log(table.tBodies);
  console.log(table.tFoot);
</script>
```

### 🖆 Adding a Row to tbody

```javascript
const table = document.querySelector("#salesTable");

const tbody = table.tBodies[0];

const row = tbody.insertRow();

row.insertCell().textContent = "Keyboard";
row.insertCell().textContent = "$50";
```

### 🖆 Creating a New tbody

```javascript
const table = document.querySelector("#salesTable");

const tbody = table.createTBody();

const row = tbody.insertRow();

row.insertCell().textContent = "Mouse";
row.insertCell().textContent = "$25";
```

### 🖆 Accessing Multiple tbody Elements

```javascript
const table = document.querySelector("#salesTable");

for (const tbody of table.tBodies) {
  console.log(tbody.rows.length);
}
```

This is useful when a table contains multiple logical groups.

# 🗁 Table Sections and Accessibility

Table sections help establish a meaningful document structure.

However, using `<thead>`, `<tbody>`, and `<tfoot>` alone does not guarantee that a table is accessible.

Consider the following practices:

* Use `<th>` for actual header cells.
* Use `scope="col"` for column headings.
* Use `scope="row"` for row headings.
* Include a meaningful `<caption>` when appropriate.
* Keep heading relationships understandable.
* Use `id` and `headers` when complex tables require explicit associations.
* Avoid using tables for page layout.
* Ensure that the table remains understandable when read sequentially.

Example:

```html
<table>
  <caption>Employee Salary Report</caption>

  <thead>
    <tr>
      <th scope="col">Employee</th>
      <th scope="col">Department</th>
      <th scope="col">Salary</th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <th scope="row">John</th>
      <td>Development</td>
      <td>$4,000</td>
    </tr>

    <tr>
      <th scope="row">Sarah</th>
      <td>Design</td>
      <td>$3,500</td>
    </tr>
  </tbody>

  <tfoot>
    <tr>
      <th scope="row" colspan="2">Total Salary</th>
      <td>$7,500</td>
    </tr>
  </tfoot>
</table>
```

# 🗁 Best Practices

* Use `<thead>` for table header rows.
* Use `<tbody>` for main data rows.
* Use `<tfoot>` for meaningful summary or footer rows.
* Use multiple `<tbody>` elements only when separate row groups make the data clearer.
* Keep the source structure consistent.
* Use semantic header cells with appropriate `scope` values.
* Use CSS to style table sections.
* Use JavaScript table properties instead of relying on fragile DOM traversal.
* Include a caption when the table needs a title or description.
* Keep tables understandable for keyboard and assistive technology users.

# 🗁 Common Mistakes

| Mistake | Correct approach |
| --- | --- |
| Using `<thead>` for ordinary data rows | Use `<tbody>` |
| Placing summary data in the wrong section | Use `<tfoot>` when it represents table summary information |
| Assuming `<tbody>` is always required in source HTML | It is optional, although explicit markup is often clearer |
| Using multiple `<thead>` elements | Use one `<thead>` and appropriate body groups |
| Using table sections only for visual styling | Use them to represent meaningful structural groups |
| Assuming sections automatically make tables accessible | Use proper headings, relationships, and captions |
| Forgetting that multiple `<tbody>` elements are possible | Use them when logical row grouping is appropriate |

# 🗁 Practical Exercises

**Exercise: Sales Report**

Create a sales report with a header, product records, and a summary footer.

**Exercise: Employee Groups**

Create a table with separate `<tbody>` elements for Development, Design, and Marketing departments.

**Exercise: Section Styling**

Apply different background colors to `<thead>`, `<tbody>`, and `<tfoot>`.

**Exercise: Dynamic Table Body**

Use JavaScript to add new records to the first `<tbody>` element.

**Exercise: Multiple Table Bodies**

Create multiple body sections and use JavaScript to count the rows in each section.

**Exercise: Accessible Report**

Build an accessible financial report using `<caption>`, `<thead>`, `<tbody>`, `<tfoot>`, and appropriate header cells.

# 🗁 Interview Questions

**What are HTML table sections?**

They are semantic elements that group table rows into header, body, and footer sections.

**What is the purpose of `<thead>`?**

It groups table header rows.

**What is the purpose of `<tbody>`?**

It groups the main data rows of a table.

**What is the purpose of `<tfoot>`?**

It groups footer or summary rows.

**Can a table contain multiple `<tbody>` elements?**

Yes. Multiple body sections can represent separate logical groups of rows.

**Can a table contain multiple `<thead>` elements?**

No. A table can have at most one `<thead>` element.

**Can a table contain multiple `<tfoot>` elements?**

No. A table can have at most one `<tfoot>` element.

**Are `<thead>`, `<tbody>`, and `<tfoot>` mandatory?**

No. They are optional in HTML, but explicit sections improve structure and maintainability.

**What is the difference between `<thead>` and `<th>`?**

`<thead>` groups header rows, while `<th>` defines an individual header cell.

**How can JavaScript access a table's body sections?**

Use the `tBodies` property.

```javascript
const table = document.querySelector("table");

console.log(table.tBodies);
```

**How can you create a new table body using JavaScript?**

Use the `createTBody()` method.

```javascript
const tbody = table.createTBody();
```

# 🗁 References

* [MDN: HTML `<thead>` Element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/thead)
* [MDN: HTML `<tbody>` Element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/tbody)
* [MDN: HTML `<tfoot>` Element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/tfoot)
* [MDN: HTMLTableElement](https://developer.mozilla.org/en-US/docs/Web/API/HTMLTableElement)
* [WHATWG: Tabular Data](https://html.spec.whatwg.org/multipage/tables.html)
