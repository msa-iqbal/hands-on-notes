# GitHub Flavored Markdown (GFM)

GitHub Flavored Markdown (GFM) is an extended version of Markdown used throughout GitHub.

It includes standard Markdown syntax along with additional features such as:

* Tables
* Task lists
* Strikethrough
* Automatic URL linking
* Alerts
* Issue and pull request references
* User mentions
* Emoji shortcodes
* Syntax-highlighted code blocks

GFM makes GitHub documentation more interactive, readable, and useful for developers.

# 🗁 What Is GitHub Flavored Markdown?

GFM is a Markdown specification based on CommonMark with additional GitHub-specific extensions.

### Standard Markdown vs GFM

| Feature | CommonMark | GitHub Flavored Markdown |
| --------- | ------------ | -------------------------- |
| Headings | Yes | Yes |
| Paragraphs | Yes | Yes |
| Bold and italic | Yes | Yes |
| Ordered lists | Yes | Yes |
| Unordered lists | Yes | Yes |
| Blockquotes | Yes | Yes |
| Fenced code blocks | Yes | Yes |
| Tables | No | Yes |
| Task lists | No | Yes |
| Strikethrough | No | Yes |
| Automatic URL linking | No | Yes |
| GitHub alerts | No | Yes |
| Issue references | No | GitHub-specific |
| User mentions | No | GitHub-specific |

**Important:** Some GitHub-specific features work only in GitHub interfaces, not in every Markdown editor or renderer.

# 🗁 GitHub Tables

GFM supports pipe-based Markdown tables.

**Syntax**

```markdown
| Name | Role | Department |
|------|------|------------|
| Rahim | Developer | IT |
| Karim | Designer | Creative |
| Hasan | Manager | HR |
```

Output:

| Name | Role | Department |
| ------ | ------ | ------------ |
| Rahim | Developer | IT |
| Karim | Designer | Creative |
| Hasan | Manager | HR |

**Column Alignment**

```markdown
| Name | Age | Salary |
|:-----|----:|------:|
| Rahim | 25 | 45000 |
| Karim | 30 | 35000 |
```

Output:

| Name | Age | Salary |
|:-----|----:|------:|
| Rahim | 25 | 45000 |
| Karim | 30 | 35000 |

Use tables for configuration references, feature comparisons, employee directories, and project documentation.

# 🗁 GitHub Task Lists

Task lists allow you to create interactive checkboxes in supported GitHub interfaces.

**Syntax**

```markdown
- [x] Create repository
- [x] Add README.md
- [ ] Configure project
- [ ] Write documentation
- [ ] Publish project
```

Output:

* [x] Create repository
* [x] Add README.md
* [ ] Configure project
* [ ] Write documentation
* [ ] Publish project

**Checkbox Syntax**

| Syntax  | Meaning         |
| ------- | --------------- |
| `- [ ]` | Incomplete task |
| `- [x]` | Completed task  |

### Nested Task Lists

```markdown
- [x] Frontend
  - [x] HTML
  - [x] CSS
  - [ ] JavaScript
- [ ] Backend
  - [ ] PHP
  - [ ] MySQL
- [ ] Deployment
```

Output:

* [x] Frontend
  * [x] HTML
  * [x] CSS
  * [ ] JavaScript
* [ ] Backend
  * [ ] PHP
  * [ ] MySQL
* [ ] Deployment

**Use cases:**

* Project roadmaps
* Development checklists
* Pull request tasks
* Issue tracking
* Learning progress
* Deployment checklists

# 🗁 Strikethrough Text

GFM supports strikethrough formatting using two tildes.

**Syntax**

```markdown
~~This task is no longer required.~~
```

Output:

~~This task is no longer required.~~

**Practical Example**

```markdown
- [x] ~~Create database schema~~
- [x] ~~Build authentication system~~
- [ ] Implement user dashboard
```

Output:

* [x] ~~Create database schema~~
* [x] ~~Build authentication system~~
* [ ] Implement user dashboard

# 🗁 Automatic URL Linking

GFM automatically recognizes many bare URLs.

**Example**

```markdown
Visit https://github.com to explore GitHub.
```

Output:

Visit <https://github.com> to explore GitHub.

**You can also use standard Markdown links**:

```markdown
[Visit GitHub](https://github.com)
```

Output:

[Visit GitHub](https://github.com)

**Recommendation:** Use descriptive Markdown links in formal documentation. Bare URLs are useful when the actual address is important.

# 🗁 GitHub Alerts

GitHub supports special blockquote syntax for displaying alerts.

Alerts highlight important information such as notes, tips, warnings, and cautions.

### Note

```markdown
> [!NOTE]
> Additional information that users should know.
```

### Tip

```markdown
> [!TIP]
> Use meaningful commit messages to make project history easier to understand.
```

### Important

```markdown
> [!IMPORTANT]
> Configure environment variables before running the application.
```

### Warning

```markdown
> [!WARNING]
> Changing production configuration may affect application availability.
```

### Caution

```markdown
> [!CAUTION]
> Deleting a production database can result in permanent data loss.
```

### Alert Types

| Alert | Purpose |
| ------- | --------- |
| `[!NOTE]` | Useful additional information |
| `[!TIP]` | Helpful advice |
| `[!IMPORTANT]` | Critical information |
| `[!WARNING]` | Potential problems |
| `[!CAUTION]` | Serious consequences |

**Important:** GitHub alert syntax is a GitHub-specific extension, not standard CommonMark. Other Markdown renderers may display these as ordinary blockquotes.

# 🗁 GitHub User Mentions

GitHub allows you to mention users using the `@username` syntax in supported contexts.

**Syntax**

```markdown
@username Please review this pull request.
```

**Example**

```markdown
@octocat Please review the documentation changes.
```

Output:

@octocat Please review the documentation changes.

A mention may notify the referenced user depending on the GitHub context and notification settings.

**Note:** Mentions are most useful in issues, pull requests, discussions, and comments. They do not function as universal notifications in every Markdown document.

# 🗁 Issue and Pull Request References

GitHub can automatically link issue and pull request references.

### Issue Reference

```markdown
Fixes #25
```

### Pull Request Reference

```markdown
Related to #42
```

### Cross-Repository Reference

```markdown
owner/repository#123
```

### Commit Reference

```markdown
See commit abc1234 for the implementation.
```

**Practical Example**

```markdown
## Related Work

- Fixes #25
- Related to #42
- See `owner/backend#18` for the API changes.
```

GitHub automatically links recognized references to the corresponding resources.

**Important:** Keywords such as `Fixes`, `Closes`, and `Resolves` can automatically close linked issues when the relevant pull request is merged into the repository's default branch, subject to GitHub's supported workflow.

# 🗁 Emoji Shortcodes

GitHub supports emoji shortcodes in many GitHub text fields.

### ☰ RESOURCES & REFERENCES

> Explore the documentation, tools, guides, and references behind the topic.

* [Emoji Shortcodes](../../19-reference/02-cheat-sheet/01-emoji-shortcodes.md)

---

**Examples**

```markdown
:smile:
:rocket:
:fire:
:heart:
:warning:
:star:
:white_check_mark:
```

**Common Emoji Shortcodes**

| Shortcode            | Emoji |
| -------------------- | ----- |
| `:smile:`            | 😄    |
| `:rocket:`           | 🚀    |
| `:fire:`             | 🔥    |
| `:heart:`            | ❤️    |
| `:warning:`          | ⚠️    |
| `:star:`             | ⭐     |
| `:white_check_mark:` | ✅     |
| `:computer:`         | 💻    |
| `:books:`            | 📚    |
| `:bug:`              | 🐛    |
| `:tada:`             | 🎉    |
| `:memo:`             | 📝    |

**Practical Example**

```markdown
# Project Documentation :books:

- :white_check_mark: Installation completed
- :rocket: Deployment started
- :bug: Bug fixes pending
```

**Note:** Emoji shortcode rendering depends on the GitHub interface or Markdown renderer.

# 🗁 Collapsible Sections

GitHub supports HTML `<details>` and `<summary>` elements for collapsible content.

**Syntax**

```html
<details>
  <summary>Installation Instructions</summary>

  1. Clone the repository.
  2. Install dependencies.
  3. Start the application.

</details>
```

**Example With Code**

```html
<details>
  <summary>Show installation commands</summary>

  ```bash
  git clone https://github.com/username/project.git
  cd project
  npm install
  npm run dev
  ```

</details>
```

**Use Cases**

* Long installation instructions
* Frequently asked questions
* Detailed code examples
* Additional technical notes
* Optional configuration details

**Note:** Collapsible HTML works in GitHub's supported Markdown contexts, but not in every Markdown renderer.

# 🗁 GitHub Relative Links

Relative links allow you to reference files within the same repository.

### Link to README

```markdown
[Read Documentation](README.md)
```

### Link to Another Folder

```markdown
[Networking Fundamentals](07-networking/01-fundamentals/readme.md)
```

### Link to an Image

```markdown
![Project Logo](assets/logo.png)
```

**Example Project Structure**

```text
my-project/
├── README.md
├── docs/
│   ├── installation.md
│   └── configuration.md
└── assets/
    └── logo.png
```

From `README.md`, you can write:

```markdown
[Installation](docs/installation.md)

[Configuration](docs/configuration.md)

![Logo](assets/logo.png)
```

Relative paths are useful because links continue to work when the repository is moved or forked, provided the referenced structure remains intact.

# 🗁 GitHub Code Blocks and Diff Highlighting

GitHub supports fenced code blocks with syntax highlighting.

### Example: JavaScript (Any supported languages)

```javascript
const user = {
    name: "Rahim",
    role: "Developer"
};

console.log(user);
```

### Example: Diff

```diff
- const port = 3000;
+ const port = 8000;
```

### Example: Terminal

```bash
npm install
npm run dev
```

Use the correct language identifier to improve readability.

# 🗁 GitHub Markdown in README Files

A GitHub README is often the first document users see when visiting a repository.

A good README usually contains:

1. Project title
2. Project description
3. Features
4. Technologies used
5. Installation instructions
6. Usage examples
7. Project structure
8. Screenshots
9. Contribution guidelines
10. License

**Example: README.md**

````markdown
# My Web Application

A simple web application built with modern technologies.

## Features

- User authentication
- Responsive interface
- REST API integration
- Database management

## Technologies

| Technology | Purpose |
|------------|---------|
| HTML | Structure |
| CSS | Styling |
| JavaScript | Application logic |
| PHP | Backend |
| MySQL | Database |

## Installation

```bash
git clone https://github.com/username/project.git
cd project
npm install
npm run dev
```

## Project Status

* [x] Initial setup
* [x] Database configuration
* [ ] Authentication
* [ ] Deployment

## License

This project is licensed under the MIT License.

````

# 🗁 GitHub Markdown in Issues and Pull Requests

Markdown helps structure bug reports, feature requests, and code reviews.

**Example Issue Template**

```markdown
## Bug Description

Describe the problem.

## Steps to Reproduce

1. Open the application.
2. Navigate to the login page.
3. Submit invalid credentials.

## Expected Behavior

An appropriate validation message should appear.

## Actual Behavior

The application displays an unexpected error.

## Environment

- Browser: Chrome
- Operating System: Linux
- Application Version: 1.0.0
```

**Example Pull Request Description**

```markdown
## Summary

Describe the changes introduced by this pull request.

## Changes

- Added user authentication.
- Improved validation.
- Fixed login errors.

## Testing

- [x] Unit tests passed.
- [x] Manual testing completed.
- [ ] Performance testing completed.

## Related Issue

Fixes #25
```

# 🖆 GitHub Markdown Limitations

Not every GitHub-specific feature works everywhere.

| Feature              | README            | Issues    | Pull Requests |
| -------------------- | ----------------- | --------- | ------------- |
| Headings             | Yes               | Yes       | Yes           |
| Tables               | Yes               | Yes       | Yes           |
| Task lists           | Yes               | Yes       | Yes           |
| Alerts               | Yes               | Yes       | Yes           |
| User mentions        | Context-dependent | Yes       | Yes           |
| Issue references     | Yes               | Yes       | Yes           |
| Collapsible sections | Supported         | Supported | Supported     |
| Emoji shortcodes     | Supported         | Supported | Supported     |

The exact behavior can depend on the GitHub interface, repository context, and sanitization rules.

# 🖆 Common Mistakes

| Mistake | Problem | Solution |
| --------- | --------- | ---------- |
| Using GFM extensions in ordinary Markdown tools | Features may not render | Check renderer support |
| Incorrect task list syntax | Checkboxes may not appear | Use `- [ ]` or `- [x]` |
| Invalid alert heading | Alert may appear as plain text | Use supported `[!TYPE]` syntax |
| Incorrect relative path | Link or image fails | Verify file location |
| Invalid issue reference | No automatic link | Use the correct issue number and repository |
| Expecting mentions everywhere | No notification may occur | Use mentions in supported GitHub contexts |
| Excessive alerts | Important content loses emphasis | Use alerts selectively |

# 🖆 Best Practices

1. Use GFM features when publishing documentation on GitHub.
2. Keep README files structured and informative.
3. Use task lists to track project progress.
4. Use alerts for genuinely important information.
5. Use relative links for repository documentation.
6. Use tables for structured comparisons.
7. Use issue references to connect related development work.
8. Use collapsible sections for lengthy optional content.
9. Use descriptive headings and consistent formatting.
10. Test Markdown rendering before publishing.
11. Avoid relying on GitHub-specific features when portability is required.
12. Keep project documentation updated with the actual application.
