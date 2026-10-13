# Alfredo Martinez — Portfolio & Blog

Personal portfolio and blog built with [Astro](https://astro.build/).

## Features

- **Blazing Fast**: Generates zero-JS static HTML for maximum speed, accessibility, and SEO.
- **Markdown Blog**: Content collections powered by Astro for writing blog posts in standard Markdown or MDX with type-safe frontmatter schema.
- **Responsive Dark Theme**: Mobile-friendly dark mode interface matching custom branding.
- **Automated Deployment**: GitHub Actions workflow automatically builds and publishes to GitHub Pages on every push to `main`.

---

## Getting Started

### 1. Install dependencies
```bash
npm install
```

### 2. Start the local development server
```bash
npm run dev
```
Open [http://localhost:4321](http://localhost:4321) in your browser.

### 3. Build for production
```bash
npm run build
```
The static output will be generated in the `dist/` directory.

### 4. Type & Astro validation
```bash
npm run check
```

---

## Writing a New Blog Post

To add a new blog post, simply create a new `.md` file inside `src/content/blog/` (e.g. `src/content/blog/my-new-post.md`):

```markdown
---
title: "Your Post Title Here"
description: "A short 1-2 sentence summary of what this article covers."
pubDate: 2026-10-15
tags: ["data", "operations", "python"]
---

Write your article content here using standard Markdown!

## Section Title

You can use:
- Bullet points
- Code blocks with syntax highlighting
- Images and links
- Blockquotes
```

Any new post is automatically formatted, styled, and listed on the `/blog` index and homepage!

---

## Project Structure

```text
├── .github/
│   └── workflows/
│       └── deploy.yml          # Automated GitHub Pages deployment
├── public/                     # Static assets (favicons, social icons, images)
├── src/
│   ├── components/
│   │   └── Navbar.astro        # Sticky navigation bar with page links & social icons
│   ├── content/
│   │   └── blog/               # Markdown blog posts
│   ├── layouts/
│   │   ├── Layout.astro        # Base site layout with meta tags & fonts
│   │   └── BlogPostLayout.astro# Article layout for reading posts
│   ├── pages/
│   │   ├── index.astro         # Homepage (portfolio sections + latest posts)
│   │   └── blog/
│   │       ├── index.astro     # Blog archive listing all posts
│   │       └── [...slug].astro # Dynamic post renderer
│   ├── styles/
│   │   └── styles.css          # Design system & dark theme styling
│   └── content.config.ts       # Type-safe blog schema definition
├── astro.config.mjs            # Astro configuration
└── package.json
```

---

## GitHub Pages Setup

This repository includes `.github/workflows/deploy.yml` which deploys automatically.

To ensure GitHub Pages uses this workflow:
1. Go to your repository on GitHub: **Settings** &rarr; **Pages**.
2. Under **Build and deployment** &rarr; **Source**, select **GitHub Actions**.
3. Push to `main`, and your site will be built and deployed automatically!
