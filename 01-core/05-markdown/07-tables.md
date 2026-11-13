# Markdown Tables

Markdown tables allow you to organize structured information into rows and columns. They are useful for presenting configuration details, comparison data, feature lists, project information, and technical documentation.

Tables are supported by GitHub Flavored Markdown (GFM) and many other Markdown implementations, but they are not part of the original CommonMark specification.

**Basic Table Syntax**

A Markdown table consists of:

1. A header row.
2. A separator row.
3. One or more data rows.

Syntax:

```markdown
| Name | Age | Department |
|------|-----|------------|
| Rahim | 25 | IT |
| Karim | 30 | HR |
| Hasan | 28 | Accounts |
```

Output:

| Name | Age | Department |
|------|-----|------------|
| Rahim | 25 | IT |
| Karim | 30 | HR |
| Hasan | 28 | Accounts |

**Explanation:**

* `|` separates columns.
* The first row defines column headings.
* The second row separates the header from the data.
* Each subsequent row represents a record.
* The number of columns should remain consistent.

# 🗁 Understanding Table Components

Consider this example:

```markdown
| Employee ID | Employee Name | Salary |
|-------------|---------------|--------|
| EMP-001 | Rahim | 25000 |
| EMP-002 | Karim | 30000 |
```

| Component       | Purpose                  |                  |
| --------------- | ------------------------ | ---------------- |
| `Employee ID`   | First column heading     |                  |
| `Employee Name` | Second column heading    |                  |
| `Salary`        | Third column heading     |                  |
| `-------------` | Header separator         |                  |
| `               | `                        | Column delimiter |
| Data rows       | Actual table information |                  |

The separator row must contain at least three hyphens per column in standard GFM table syntax.

# 🗁 Column Alignment

Markdown tables support three types of column alignment.

### 🖆 Left Alignment

Use a colon at the beginning of the separator.

```markdown
| Name | Department |
|:-----|:-----------|
| Rahim | IT |
| Karim | HR |
```

### 🖆 Center Alignment

Use colons at both ends.

```markdown
| Name | Department |
|:----:|:-----------:|
| Rahim | IT |
| Karim | HR |
```

### 🖆 Right Alignment

Use a colon at the end.

```markdown
| Product | Price |
|---------|------:|
| Laptop | 75000 |
| Monitor | 18000 |
```

### 🖆 Alignment Reference

| Syntax | Alignment |
|--------|-----------|
| `|:---|` | Left |
| `|:---:|` | Center |
| `|---:|` | Right |
| `|---|` | Default (usually left) |

**Practical recommendation:**

* Text columns: Left alignment.
* Numeric columns: Right alignment.
* Status or short labels: Center alignment.

# 🗁 Formatting Text Inside Tables

Markdown tables support many inline formatting elements.

### 🖆 Bold Text

```markdown
| Name | Status |
|------|--------|
| Rahim | **Active** |
| Karim | **Inactive** |
```

### 🖆 Italic Text

```markdown
| Technology | Description |
|------------|-------------|
| *HTML* | Markup language |
| *CSS* | Styling language |
```

### 🖆 Strikethrough

```markdown
| Task | Status |
|------|--------|
| Documentation | ~~Pending~~ |
| Testing | Completed |
```

### 🖆 Inline Code

```markdown
| Command | Description |
|---------|-------------|
| `git status` | Check repository status |
| `npm install` | Install dependencies |
```

### 🖆 Combining Formatting

```markdown
| Feature | Description |
|---------|-------------|
| **HTML** | *Markup* language |
| **JavaScript** | Programming language |
| `npm run dev` | Start development server |
```

# 🗁 Adding Links Inside Tables

You can insert hyperlinks into table cells.

```markdown
| Technology | Website |
|------------|---------|
| HTML | [MDN](https://developer.mozilla.org/en-US/docs/Web/HTML) |
| GitHub | [GitHub](https://github.com) |
| Markdown | [Guide](https://www.markdownguide.org/) |
```

Output:

| Technology | Website |
| ------------ | --------- |
| HTML | [MDN](https://developer.mozilla.org/en-US/docs/Web/HTML) |
| GitHub | [GitHub](https://github.com) |
| Markdown | [Guide](https://www.markdownguide.org/) |

# 🗁 Adding Images Inside Tables

Some Markdown renderers support images inside table cells.

```markdown
| Technology | Logo |
|------------|------|
| HTML | ![HTML Logo](images/html.png) |
| CSS | ![CSS Logo](images/css.png) |
```

**Important:** Images can make tables very wide. Consider using short labels or separate image sections when appropriate.

# 🗁 Escaping Special Characters

The pipe character (`|`) separates table columns. If you need to display a literal pipe inside a cell, escape it with a backslash.

**Example**

```markdown
| Expression | Meaning |
|------------|---------|
| `A \| B` | A OR B |
| `X \| Y` | X OR Y |
```

Output

| Expression | Meaning |
|------------|---------|
| `A \| B` | A OR B |
| `X \| Y` | X OR Y |

Without escaping, the pipe may be interpreted as a column separator.

# 🗁 Empty Cells

You can leave a cell empty when information is unavailable or not applicable.

```markdown
| Employee | Department | Phone |
|----------|------------|-------|
| Rahim | IT | 01700000001 |
| Karim | HR | |
| Hasan | | 01700000003 |
```

Output

| Employee | Department | Phone |
| ---------- | ------------ | ------- |
| Rahim | IT | 01700000001 |
| Karim | HR | |
| Hasan | | 01700000003 |

For better clarity, you may use `N/A`, `Not Available`, or `—` instead of leaving important fields empty.

# 🗁 Multiple Lines Inside Table Cells

Standard Markdown tables do not provide portable support for arbitrary multiline cell content.

Some renderers, including GitHub, allow HTML `<br>` tags for line breaks inside cells.

Example:

```markdown
| Name | Skills |
|------|--------|
| Rahim | HTML<br>CSS<br>JavaScript |
| Karim | PHP<br>MySQL |
```

Output

| Name  | Skills                    |
| ----- | ------------------------- |
| Rahim | HTML<br>CSS<br>JavaScript |
| Karim | PHP<br>MySQL              |

**Note:** HTML support varies between Markdown renderers. Use line breaks only when you know the target platform supports them.

# 🗁 HTML Tables as an Alternative

When Markdown table syntax is insufficient, HTML tables can provide additional control.

### 🖆 Example

```html
<table>
  <thead>
    <tr>
      <th>Employee</th>
      <th>Department</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Rahim</td>
      <td>IT</td>
    </tr>
    <tr>
      <td>Karim</td>
      <td>HR</td>
    </tr>
  </tbody>
</table>
```

HTML tables can support more complex structures, such as:

* Cell merging with `rowspan`.
* Column merging with `colspan`.
* Additional HTML attributes.
* More complex cell layouts.

**Important:** HTML support depends on the Markdown renderer and its security configuration. Some platforms sanitize or restrict HTML.

# 🖆 Practical Example: Employee Information

```markdown
| S/L | Employee ID | Employee Name | Department | Designation | Salary |
|----:|-------------|---------------|------------|-------------|-------:|
| 1 | EMP-001 | Rahim | IT | Developer | 45000 |
| 2 | EMP-002 | Karim | HR | Executive | 35000 |
| 3 | EMP-003 | Hasan | Accounts | Officer | 40000 |
| 4 | EMP-004 | Noman | IT | Network Engineer | 42000 |
```

Output:

| S/L | Employee ID | Employee Name | Department | Designation | Salary |
| ----: | ------------- | --------------- | ------------ | ------------- | -------: |
| 1 | EMP-001 | Rahim | IT | Developer | 45000 |
| 2 | EMP-002 | Karim | HR | Executive | 35000 |
| 3 | EMP-003 | Hasan | Accounts | Officer | 40000 |
| 4 | EMP-004 | Noman | IT | Network Engineer | 42000 |

This structure is useful for employee directories, payroll documentation, and HR reports.

# 🖆 Best Practices

1. Keep table headings short and descriptive.
2. Use consistent column counts.
3. Align numeric values to the right.
4. Align ordinary text to the left.
5. Use center alignment for short labels when useful.
6. Avoid unnecessary columns.
7. Keep cell content concise.
8. Escape literal pipe characters.
9. Use meaningful values instead of unexplained empty cells.
10. Test tables in the Markdown renderer where they will be published.
11. Prefer Markdown tables for simple data and HTML tables only when additional structure is required.
