# HTML Table Rows

The HTML `<tr>` element defines a row inside an HTML table.

A table row contains cells that represent individual pieces of data. These cells are usually created with `<td>` for regular data or `<th>` for header information.

Table rows organize related values horizontally, making structured information easier to read and understand.

# 🗁 Introduction to Table Rows

An HTML table is organized into rows and columns.

Each row represents a horizontal group of related cells.

For example, an employee table might contain:

| ID | Name | Department |
| --- | --- | --- |
| 101 | John | Development |
| 102 | Sarah | Design |
| 103 | Alex | Marketing |

Each horizontal line represents a table row.

The `<tr>` element is responsible for defining these rows.

# 🗁 The tr Element

The `<tr>` element stands for **Table Row**.

It acts as a container for table cells.

### 🖆 Syntax

```html
<tr>
  <td>Cell 1</td>
  <td>Cell 2</td>
  <td>Cell 3</td>
</tr>
```

### 🖆 Example

```html
<table>
  <tr>
    <td>101</td>
    <td>John</td>
    <td>Developer</td>
  </tr>
</table>
```

The browser displays one row containing three cells.

### 🖆 Important Characteristics

* `<tr>` must be used within a table structure.
* It contains table cells such as `<td>` and `<th>`.
* A row can contain multiple cells.
* Rows can contain different numbers of cells, although consistent column structures are generally preferable.
* CSS can control row appearance.
* JavaScript can dynamically create, update, and remove rows.

# 🗁 Creating Table Rows

A basic table can contain multiple `<tr>` elements.

```html
<table>
  <tr>
    <td>ID</td>
    <td>Name</td>
    <td>Role</td>
  </tr>

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

  <tr>
    <td>103</td>
    <td>Alex</td>
    <td>Administrator</td>
  </tr>
</table>
```

The table contains four rows, each representing a separate group of information.

# 🗁 Rows with Header and Data Cells

A table row can contain both header cells and ordinary data cells.

Use `<th>` for cells that identify or describe related rows or columns, and `<td>` for regular data.

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

The first row contains column headings. The following rows contain data.

The browser typically displays header cells in bold and centers their text by default, although CSS can change this presentation.

# 🗁 Multiple Table Rows

A table can contain as many rows as its data requires.

Example: Student records.

```html
<table>
  <tr>
    <th>Student ID</th>
    <th>Name</th>
    <th>Course</th>
    <th>Score</th>
  </tr>

  <tr>
    <td>ST001</td>
    <td>John</td>
    <td>HTML</td>
    <td>85</td>
  </tr>

  <tr>
    <td>ST002</td>
    <td>Sarah</td>
    <td>CSS</td>
    <td>92</td>
  </tr>

  <tr>
    <td>ST003</td>
    <td>Alex</td>
    <td>JavaScript</td>
    <td>88</td>
  </tr>
</table>
```

Each student is represented by a separate row.

# 🗁 Table Rows with thead, tbody, and tfoot

HTML provides grouping elements to organize table rows into logical sections.

| Element | Purpose |
| --- | --- |
| `<thead>` | Groups header rows |
| `<tbody>` | Groups the main data rows |
| `<tfoot>` | Groups footer or summary rows |

Example:

```html
<table>
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
      <td>$1,500</td>
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
      <td>$1,750</td>
    </tr>
  </tfoot>
</table>
```

These grouping elements improve document structure and make tables easier to style and maintain.

A table can contain multiple `<tbody>` elements when its data is divided into separate row groups.

# 🗁 Row Spanning

The `rowspan` attribute allows a table cell to occupy multiple rows.

This is useful when multiple rows share the same category or value.

### 🖆 Basic Example

```html
<table>
  <tr>
    <th>Department</th>
    <th>Employee</th>
  </tr>

  <tr>
    <td rowspan="2">Development</td>
    <td>John</td>
  </tr>

  <tr>
    <td>Alex</td>
  </tr>

  <tr>
    <td>Design</td>
    <td>Sarah</td>
  </tr>
</table>
```

The `Development` cell spans two rows.

### 🖆 Understanding rowspan

```html
<td rowspan="2">Development</td>
```

This tells the browser that the cell occupies two rows in the same column position.

The following row does not need another cell in that occupied position.

# 🗁 Styling Table Rows with CSS

CSS provides several properties and selectors for styling table rows.

### 🖆 Basic Row Styling

```html
<table class="employee-table">
  <tr>
    <th>ID</th>
    <th>Name</th>
    <th>Role</th>
  </tr>

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
</table>
```

```css
.employee-table {
  width: 100%;
  border-collapse: collapse;
}

.employee-table tr {
  border-bottom: 1px solid #ddd;
}

.employee-table th,
.employee-table td {
  padding: 12px;
  text-align: left;
}
```

### 🖆 Alternating Row Colors

Use the `:nth-child()` pseudo-class to apply different styles to alternating rows.

```css
.employee-table tbody tr:nth-child(even) {
  background-color: #f2f2f2;
}

.employee-table tbody tr:nth-child(odd) {
  background-color: #ffffff;
}
```

This styling technique is commonly called zebra striping.

### 🖆 Hover Effect

```css
.employee-table tbody tr:hover {
  background-color: #e8f0fe;
}
```

The row background changes when the pointer moves over it.

### 🖆 Highlight a Specific Row

```html
<table>
  <tr>
    <td>John</td>
    <td>Developer</td>
  </tr>

  <tr class="active-row">
    <td>Sarah</td>
    <td>Designer</td>
  </tr>
</table>
```

```css
.active-row {
  background-color: #dff0d8;
  font-weight: bold;
}
```

# 🗁 Selecting Rows with CSS

CSS selectors make it possible to target specific table rows.

| Selector | Purpose |
| --- | --- |
| `tr` | Selects all table rows |
| `tr:first-child` | Selects the first row among its siblings |
| `tr:last-child` | Selects the last row among its siblings |
| `tr:nth-child(2)` | Selects the second row among its siblings |
| `tr:nth-child(even)` | Selects rows at even sibling positions |
| `tr:nth-child(odd)` | Selects rows at odd sibling positions |
| `tbody tr` | Selects rows inside a table body |
| `tr:hover` | Selects a row while the pointer hovers over it |

Example:

```css
tbody tr:first-child {
  background-color: #eee;
}

tbody tr:last-child {
  border-bottom: 2px solid #333;
}

tbody tr:nth-child(3) {
  font-weight: bold;
}
```

Note that `:nth-child()` counts an element's position among its siblings, not a special table-row index. When selecting rows inside a particular section, scope the selector to that section.

# 🗁 Table Rows with JavaScript

JavaScript can create and manipulate table rows dynamically.

This is useful for dashboards, administrative panels, data tables, and applications that receive data from APIs.

### 🖆 Accessing Existing Rows

```html
<table id="employeeTable">
  <tr>
    <td>101</td>
    <td>John</td>
  </tr>

  <tr>
    <td>102</td>
    <td>Sarah</td>
  </tr>
</table>

<script>
  const table = document.querySelector("#employeeTable");

  const rows = table.rows;

  console.log(rows.length);
</script>
```

The `rows` collection provides access to the table's rows.

### 🖆 Creating a New Row

```javascript
const table = document.querySelector("#employeeTable");

const row = table.insertRow();

const idCell = row.insertCell();
const nameCell = row.insertCell();

idCell.textContent = "103";
nameCell.textContent = "Alex";
```

### 🖆 Removing a Row

```javascript
const table = document.querySelector("#employeeTable");

table.deleteRow(1);
```

The index is zero-based, so index `1` removes the second row.

### 🖆 Adding Rows from an Array

```javascript
const employees = [
  { id: 101, name: "John", role: "Developer" },
  { id: 102, name: "Sarah", role: "Designer" },
  { id: 103, name: "Alex", role: "Administrator" }
];

const tableBody = document.querySelector("#employeeTable tbody");

employees.forEach((employee) => {
  const row = tableBody.insertRow();

  row.insertCell().textContent = employee.id;
  row.insertCell().textContent = employee.name;
  row.insertCell().textContent = employee.role;
});
```

Using `textContent` for plain text values avoids interpreting those values as HTML.

# 🗁 Responsive Table Rows

On small screens, tables with many columns can become difficult to read.

A practical solution is to place the table inside a horizontally scrollable container.

```html
<div class="table-container">
  <table>
    <thead>
      <tr>
        <th>ID</th>
        <th>Name</th>
        <th>Email</th>
        <th>Department</th>
        <th>Position</th>
      </tr>
    </thead>

    <tbody>
      <tr>
        <td>101</td>
        <td>John</td>
        <td>john@example.com</td>
        <td>Development</td>
        <td>Frontend Developer</td>
      </tr>
    </tbody>
  </table>
</div>
```

```css
.table-container {
  width: 100%;
  overflow-x: auto;
}

.table-container table {
  width: 100%;
  min-width: 650px;
  border-collapse: collapse;
}

.table-container th,
.table-container td {
  padding: 12px;
  border: 1px solid #ddd;
}
```

The container allows horizontal scrolling when the table is wider than the available screen.

# 🗁 Best Practices

* Use `<tr>` to represent each logical row of data.
* Use `<th>` for header cells and `<td>` for ordinary data cells.
* Group rows with `<thead>`, `<tbody>`, and `<tfoot>` where appropriate.
* Keep the number and meaning of columns consistent across rows.
* Use `rowspan` only when it accurately represents the data relationships.
* Use CSS for row styling instead of obsolete HTML attributes.
* Use `textContent` when inserting untrusted plain text with JavaScript.
* Keep wide tables usable on smaller screens.
* Do not rely on color alone to communicate important row states.
* Preserve logical reading order for assistive technologies.

# 🗁 Common Mistakes

| Mistake | Correct approach |
| --- | --- |
| Using `<tr>` outside a table structure | Place rows inside a table |
| Putting ordinary text directly inside `<tr>` | Wrap content in `<td>` or `<th>` |
| Using `<td>` for every cell | Use `<th>` for header cells |
| Duplicating cells covered by `rowspan` | Account for cells already spanning rows |
| Using inconsistent columns without a reason | Maintain a logical row structure |
| Using JavaScript `innerHTML` for untrusted values | Prefer `textContent` |
| Making wide tables unusable on mobile | Use a responsive scrolling container |

# 🗁 Practical Exercises

**Exercise: Employee Records**

Create a table with five employee records. Each row should contain an ID, name, department, and salary.

**Exercise: Row Styling**

Use CSS to create alternating row colors and a hover effect.

**Exercise: Department Grouping**

Use `rowspan` to group employees belonging to the same department.

**Exercise: Dynamic Rows**

Create an employee table and use JavaScript to add a new row when a user clicks an Add Employee button.

**Exercise: Delete Rows**

Add a delete button to each row and remove the selected row using JavaScript.

**Exercise: Responsive Table**

Create a table with several columns and make it horizontally scrollable on smaller screens.

# 🗁 Interview Questions

**What is the `<tr>` element?**

The `<tr>` element defines a row inside an HTML table.

**What elements can be placed inside `<tr>`?**

Typically, `<td>` and `<th>` elements.

**Can a table contain multiple `<tr>` elements?**

Yes. Each row represents a separate horizontal group of table cells.

**What is the difference between `<thead>`, `<tbody>`, and `<tfoot>`?**

They group table rows into header, body, and footer sections.

**What is rowspan?**

It allows a table cell to occupy multiple rows.

**How can you select alternating table rows with CSS?**

Use the `:nth-child()` pseudo-class.

```css
tbody tr:nth-child(even) {
  background-color: #eee;
}
```

**How can JavaScript create a new table row?**

Use the `insertRow()` method.

```javascript
const row = table.insertRow();
```

**How can you remove a table row using JavaScript?**

Use the `deleteRow()` method with a zero-based row index.

**How can you make table rows usable on mobile devices?**

Place the table inside a container with `overflow-x: auto` when horizontal scrolling is appropriate.

# 🗁 References

* [MDN: HTML `<tr>` Element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/tr)
* [MDN: HTML `<td>` Element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/td)
* [MDN: HTML `<th>` Element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/th)
* [MDN: HTMLTableElement](https://developer.mozilla.org/en-US/docs/Web/API/HTMLTableElement)
* [MDN: HTMLTableRowElement](https://developer.mozilla.org/en-US/docs/Web/API/HTMLTableRowElement)
* [WHATWG: Tabular Data](https://html.spec.whatwg.org/multipage/tables.html)
