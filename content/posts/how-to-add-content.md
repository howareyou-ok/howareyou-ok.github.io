---
title: "How to Add New Content to the Website"
date: 2024-01-16T14:30:00+08:00
tags: ["tutorial", "hugo", "markdown"]
---

This article will teach you how to add new content to the B-GG website.

## Adding New Articles

To add a new article, simply create a new Markdown file in the `content/posts/` directory.

### File Naming

We recommend using English file names, for example:
- `my-first-tutorial.md`
- `software-guide-2024.md`
- `security-tips.md`

### Article Format

Each article needs to include Front Matter (article header information):

```markdown
---
title: "Article Title"
date: 2024-01-16T14:30:00+08:00
tags: ["tag1", "tag2"]
---

Article content goes here...
```

### Front Matter Explanation

- `title`: Article title
- `date`: Publication date (ISO 8601 format)
- `tags`: Article tags (optional)

## Adding New Pages

To add new standalone pages (like "About Us"), create a `.md` file in the `content/` directory:

```markdown
---
title: "Page Title"
menu: "main"  # Add to main menu
---

Page content...
```

## Markdown Syntax

Standard Markdown syntax is supported:

### Headers
```markdown
# Level 1 Header
## Level 2 Header
### Level 3 Header
```

### Lists
```markdown
- Unordered list item 1
- Unordered list item 2

1. Ordered list item 1
2. Ordered list item 2
```

### Code
```markdown
`inline code`

```language
code block
```
```

### Links and Images
```markdown
[Link text](https://example.com)
![Image description](image.jpg)
```

## Automatic Publishing

When you commit Markdown files to the `main` branch of your GitHub repository, GitHub Actions will automatically:

1. Build the Hugo website
2. Deploy to GitHub Pages
3. Your new content will go live automatically

It's that simple! Just write Markdown, and let the automated process handle the rest.