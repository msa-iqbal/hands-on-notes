# HTML Table Fundamentals

HTML tables are used to represent structured, tabular data in rows and columns.

They are useful when displaying information that has relationships between multiple data points, such as:

* Employee records
* Product inventories
* Financial reports
* Student results
* Schedules
* Comparison data
* Statistical information

HTML provides the `<table>` element as the container for tabular data.

A basic table consists of rows (`<tr>`) and cells (`<td>`).

# 🗁 Introduction to HTML Tables

An HTML table organizes information into a two-dimensional structure.

Each row represents a record or group of related information, while each column represents a particular category of data.

Example:

| ID | Name | Role |
| --- | --- | --- |
| 101 | John | Developer |
| 102 | Sarah | Designer |
| 103 | Alex | Administrator |

The corresponding HTML structure uses:

* `<table>` to define the table.
* `<tr>` to define a row.
* `<td>` to define a standard data cell.

# 🗁 Basic Table Structure

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
</table>
```

The browser displays the information in rows and columns.

### 🖆 Visual Structure

```text
<table>
  ├── <tr>
  │     ├── <td>ID</td>
  │     ├── <td>Name</td>
  │     └── <td>Role</td>
  │
  ├── <tr>
  │     ├── <td>101</td>
  │     ├── <td>John</td>
  │     └── <td>Developer</td>
  │
  └── <tr>
        ├── <td>102</td>
        ├── <td>Sarah</td>
        └── <td>Designer</td>
```

# 🗁 Understanding Table Elements

HTML provides several elements for organizing table content.

| Element | Purpose |
| --- | --- |
| `<table>` | Defines the table |
| `<tr>` | Defines a table row |
| `<td>` | Defines a standard data cell |
| `<th>` | Defines a header cell |
| `<caption>` | Provides a table title or description |
| `<thead>` | Groups header rows |
| `<tbody>` | Groups the main data rows |
| `<tfoot>` | Groups footer or summary rows |
| `<colgroup>` | Groups columns for styling |
| `<col>` | Represents a column within a column group |

These elements allow developers to create meaningful and accessible table structures.

The individual row, cell, and header elements are covered in the following chapters.

# 🗁 Table Attributes

The `<table>` element supports global HTML attributes and CSS styling.

Historically, developers used HTML attributes such as `border`, `cellpadding`, `cellspacing`, `width`, and `align` to control table appearance.

These presentational attributes are obsolete or deprecated in modern HTML authoring. Use CSS instead.

### 🖆 Example of Modern Table Styling

```html
<table class="employee-table">
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
```

# 🗁 Adding Borders

CSS controls table borders.

```html
<table class="employee-table">
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
</table>
```

```css
.employee-table {
  width: 100%;
  border-collapse: collapse;
}

.employee-table td {
  border: 1px solid #ccc;
  padding: 10px;
}
```

### 🖆 Understanding border-collapse

The `border-collapse` property determines how adjacent table cell borders are displayed.

**Separate borders:**

```css
table {
  border-collapse: separate;
}
```

Adjacent cells maintain separate borders.

**Collapsed borders:**

```css
table {
  border-collapse: collapse;
}
```

Adjacent borders are combined into a shared border.

For many traditional data tables, `border-collapse: collapse` provides a clean appearance.

# 🗁 Table Width and Styling

CSS can control the dimensions, spacing, alignment, and appearance of tables.

```css
table {
  width: 100%;
  border-collapse: collapse;
}

td {
  padding: 12px;
  text-align: left;
  vertical-align: middle;
  border: 1px solid #ddd;
}
```

### 🖆 Common CSS Properties

| Property | Purpose |
| --- | --- |
| `width` | Controls table width |
| `max-width` | Sets the maximum width |
| `border` | Defines borders |
| `border-collapse` | Controls border collapsing |
| `border-spacing` | Controls spacing between separate cell borders |
| `padding` | Adds space inside cells |
| `text-align` | Controls horizontal text alignment |
| `vertical-align` | Controls vertical alignment within cells |
| `background-color` | Sets the background color |

### 🖆 Alternating Row Colors

The `:nth-child()` pseudo-class can style alternating rows.

```css
tbody tr:nth-child(even) {
  background-color: #f2f2f2;
}
```

This creates a zebra-striped table that can make rows easier to distinguish.

# 🗁 Spanning Rows and Columns

HTML tables support cells that span multiple rows or columns.

The relevant attributes are:

* `colspan` — spans multiple columns.
* `rowspan` — spans multiple rows.

### 🖆 Column Spanning

```html
<table>
  <tr>
    <td colspan="2">Personal Information</td>
  </tr>

  <tr>
    <td>Name</td>
    <td>John</td>
  </tr>
</table>
```

The first cell occupies two columns.

### 🖆 Row Spanning

```html
<table>
  <tr>
    <td rowspan="2">Developer</td>
    <td>John</td>
  </tr>

  <tr>
    <td>Alex</td>
  </tr>
</table>
```

The first cell occupies two rows.

Use spanning when it accurately represents the relationships in the data. More complex spans can make tables harder to understand and maintain.

# 🗁 HTML Tables with CSS

A complete table can combine HTML structure with CSS presentation.

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">

  <title>Employee Table</title>

  <style>
    .employee-table {
      width: 100%;
      border-collapse: collapse;
      font-family: Arial, sans-serif;
    }

    .employee-table td {
      padding: 12px;
      border: 1px solid #ddd;
      text-align: left;
    }

    .employee-table tr:nth-child(even) {
      background-color: #f5f5f5;
    }

    .employee-table tr:hover {
      background-color: #e9e9e9;
    }
  </style>
</head>
<body>

  <h1>Employee Information</h1>

  <table class="employee-table">
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

</body>
</html>
```

This example demonstrates how CSS can improve readability without using obsolete table presentation attributes.

# 🗁 When to Use Tables

Use HTML tables when the content represents structured data with meaningful relationships between rows and columns.

**Good use cases:**

* Financial statements
* Product specifications
* Employee directories
* Examination results
* Statistical reports
* Pricing comparisons

**Avoid using tables for:**

* General page layouts
* Navigation menus
* Card-based interfaces
* Sidebars
* Flexible content positioning

Use CSS Flexbox or Grid for page layout instead.

# 🗁 Best Practices

* Use tables only for tabular data.
* Include meaningful column headers using `<th>`.
* Use `<caption>` when a table needs a title or description.
* Group header, body, and footer rows appropriately.
* Use `scope` on header cells when appropriate.
* Keep table structures simple and logical.
* Use CSS rather than obsolete presentation attributes.
* Provide enough spacing for readability.
* Ensure sufficient text and background contrast.
* Consider horizontal scrolling for wide tables on small screens.
* Use semantic table markup to support assistive technologies.

# 🗁 Common Mistakes

| Mistake | Correct approach |
| --- | --- |
| Using tables for page layout | Use CSS Grid or Flexbox |
| Using `<td>` for every cell | Use `<th>` for header cells |
| Using HTML presentation attributes | Use CSS |
| Creating unnecessarily complex spans | Keep table structures simple |
| Omitting meaningful table headers | Provide appropriate header cells |
| Making wide tables unusable on mobile | Add a responsive scrolling container |
| Using tables without meaningful relationships | Use lists or other semantic elements when appropriate |

# 🗁 Practical Exercises

**Exercise: Basic Employee Table**

Create a table containing employee ID, name, department, and salary.

**Exercise: Product Comparison**

Create a product comparison table with product names, prices, features, and availability.

**Exercise: Student Results**

Create a student result table containing names, subjects, marks, and grades.

**Exercise: Spanning Cells**

Build a timetable that uses `rowspan` and `colspan`.

**Exercise: Table Styling**

Use CSS to add borders, padding, alternating row colors, and hover effects.

**Exercise: Responsive Table**

Place a wide table inside a horizontally scrollable container and test it on a small screen.

# 🗁 Interview Questions

**What is an HTML table?**

An HTML table represents structured data in rows and columns.

**Which element defines an HTML table?**

The `<table>` element.

**What is the difference between `<tr>` and `<td>`?**

`<tr>` defines a table row, while `<td>` defines a standard data cell.

**What is the difference between `<td>` and `<th>`?**

`<td>` represents ordinary data, while `<th>` represents a header cell.

**What is colspan?**

`colspan` specifies how many columns a cell should span.

**What is rowspan?**

`rowspan` specifies how many rows a cell should span.

**What is border-collapse?**

It is a CSS property that controls whether adjacent table cell borders are separated or combined.

**Should HTML tables be used for website layouts?**

No. CSS Grid and Flexbox are better suited to page layouts. Tables should represent tabular data.

**How can you make a table responsive?**

A common approach is to place the table inside a container with `overflow-x: auto` so users can scroll horizontally when necessary.

# 🗁 References

* [MDN: HTML Table](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/table)
* [MDN: HTML Table Basics](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Structuring_content/HTML_table_basics)
* [MDN: CSS Table Layout](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_table)
* [WHATWG: Tabular Data](https://html.spec.whatwg.org/multipage/tables.html)
