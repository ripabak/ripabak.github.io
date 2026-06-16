# AGENTS.md – ripabak Blog Guide

## Project Overview

- **Hugo** v0.154+ (static site generator) with **PaperMod** theme
- Hosted on GitHub Pages via `.github/workflows/hugo.yaml`
- Repo: `ripabak/ripabak.github.io` (master branch)
- Local dev: `hugo server` (serves at `http://localhost:1313`)

## Directory Structure (Key Files Only)

```
.
├── hugo.yaml                  # Main config: languages, menus, PaperMod params
├── archetypes/post.md         # Frontmatter template for new posts
├── assets/images/             # All blog images, one subfolder per post slug
├── content/posts/             # ALL blog posts (flat, no subdirectories)
│   ├── {slug}.md              # Indonesian version (default language)
│   └── {slug}.en.md           # English version
├── i18n/                      # UI translation strings (id.yaml, en.yaml)
├── layouts/
│   ├── index.html             # Custom homepage (posts + projects carousel)
│   └── partials/extend_head.html
└── themes/PaperMod/           # Git submodule – do NOT modify
```

- `archetypes/post.md` is the canonical frontmatter template. Run `hugo new --kind post content/posts/{slug}.md` to scaffold a new post.
- `assets/images/{slug}/` holds all images for a post. Images are referenced as `images/{slug}/file.png` in markdown.
- `static/` is only for Partytown JS – **not** for blog assets.
- `assets/css/extended/` has custom CSS overrides (`custom.css`, `projects.css`).

---

## Multi-Language System

| Language    | Lang Code | URL Prefix | File Pattern    | Default? |
|-------------|-----------|------------|-----------------|----------|
| Indonesian  | `id`      | `/` (root) | `{slug}.md`     | Yes      |
| English     | `en`      | `/en/`     | `{slug}.en.md`  | No       |

Language is inferred from the **filename suffix**, NOT from a frontmatter field.

Hugo auto-links translations when two files share the **exact same slug** (the part before `.md` / `.en.md`).

### Creating a Bilingual Post

1. Create the Indonesian file:
   ```
   hugo new --kind post content/posts/{My-Post-Slug}.md
   ```
2. Copy to English:
   ```
   cp content/posts/{My-Post-Slug}.md content/posts/{My-Post-Slug}.en.md
   ```
3. Translate the English version:
   - `title` → English title
   - `description` → English description
   - Body → natural English translation (not literal/word-for-word)
   - All other fields (`date`, `tags`, booleans) stay identical
   - `editPost.URL` in the `.en.md` file should still point to the `.md` file path

### Bilingual Rules (CHECKLIST)

- [ ] Same slug in both filenames
- [ ] Same `date` value in both files
- [ ] Same `tags` array in both files
- [ ] Same boolean params in both files
- [ ] `editPost.URL` in `.en.md` points to the `.md` file, not the `.en.md` file
- [ ] Body is a natural translation, not literal

If a post is intentionally Indonesian-only, omit the `.en.md` file entirely.

---

## Frontmatter Template

```yaml
---
title: "My Post Title"
date: 2026-06-15T12:00:00+07:00
tags: ["Tag1", "Tag2"]
author: "Me"
showToc: true
TocOpen: false
draft: false
hidemeta: false
comments: false
description: "Short summary – shows in meta tags and post listings."
searchHidden: false
ShowReadingTime: true
ShowBreadCrumbs: true
ShowPostNavLinks: true
ShowWordCount: true
ShowRssButtonInSectionTermList: true
UseHugoToc: true
editPost:
    URL: "https://github.com/ripabak/ripabak.github.io/content/posts/{My-Post-Slug}.md"
    Text: "Suggest Changes"
    appendFilePath: true
---
```

### Optional Fields

```yaml
canonicalURL: "https://medium.com/..."     # cross-post origin link
disableHLJS: true                          # disable highlight.js syntax coloring
cover:
    image: "images/{slug}/banner.png"      # path relative to assets/
    alt: "Alt text for image"
    caption: "Caption under cover image"
    relative: false                        # always false (no page bundles)
    hidden: false
```

### Field Notes

| Field | Rule |
|-------|------|
| `date` | Always `+07:00` timezone (WIB/Indonesia). Same value in both language files. Slugs with hyphens-separated format. |
| `tags` | Must match between both language files. Use `"Projects"` to feature on the homepage carousel. |
| `description` | Becomes both `<meta description>` and the post summary in list/card views. 1-2 sentences. |
| `cover.image` | Path **relative to `assets/`**. Always starts with `images/`. One folder per post slug. |
| `cover.relative` | Always `false` (the site uses flat files, not page bundles). |
| `editPost.URL` | In `.en.md`, point to the `.md` file (Indonesian primary). In `.md`, point to itself. |
| `draft` | Set to `true` to hide from production. |

---

## Writing Style Guide

### Core Tone (Applies to Both Languages)

- **First-person, conversational, personal.** Write like you're talking to a friend over coffee.
- **Casual & informal.** No academic or corporate tone. Short sentences. Natural flow.
- **Code-switching in Indonesian is normal and encouraged.** English technical terms, filler phrases like "btw", "so far", "at least", "it should be make sense" belong in Indonesian posts.
- **Self-deprecating humor** and personal anecdotes. Don't take yourself too seriously.
- **Reflective.** Include why you built something, what you learned, what you'd do differently.
- **Close with a friendly sign-off.** "Makasih udah baca, semoga membantu :)" / "Thank you for reading :)"

### Indonesian Style

- **Pronouns:** `aku` (I), `kamu` (you) – casual; `saya`, `anda` also appear but `aku`/`kamu` dominates.
- **Colloquial particles:** `dong`, `nih`, `ya`, `sih`, `deh`, `kan`, `lho`, `wkwk`, `kok`
- **Opening phrases:** "Jadi gini...", "Beberapa waktu yang lalu, aku..."
- **Mid-sentence English:** "sounds impossible right?", "it should be make sense right (?)", "so disturbing", "its okay", "whatt? how?"
- **Example tone (from existing posts):**
  - "Aku pernah dengar cerita dari seorang teman dekat aku, dia pernah berada di situasi yang sulit, dia di palak sama pihak tertentu, aku gabisa sebutin pihak nya ya disini wkwk."
  - "Di sini bisa di pahami lah ya, teman aku itu adalah korban, dimana dia bisa dapat keadilan di situasi seperti itu (?)."
  - "Tenang hasil video nya akan disimpan di browsermu sendiri kok, tidak akan disimpan di server."

### English Style

- **Clean, simple English.** Not a literal translation. Make it read naturally to an English speaker.
- **Pronouns:** `I`, `you` (never `we` in academic sense, never passive voice).
- **Example tone (from existing posts):**
  - "So, this website is my personal space that I created as an archive of knowledge and reflection."
  - "I once heard a story from a close friend of mine. He was in a difficult situation, being extorted by certain parties—I can't mention who here haha."
  - "Don't worry, the video results will be stored in your own browser, not on a server."

### Recommended Post Structure

1. **Personal hook/story** – Why this matters to you, how you got into it (1-2 short paragraphs)
2. **What you built / discovered** – The core content, problem statement
3. **How it works / Features / Steps** – With code blocks, screenshots, or numbered steps
4. **Reflection / Closing** – What you took away, link to GitHub/tool, friendly sign-off

### Content Type Patterns

| Type | Typical Tags | Structure |
|------|-------------|-----------|
| Project showcase | `["Projects", "AI", "LLM", ...]` | Backstory → Problem → How it works → Tech stack → Screenshots → GitHub link |
| Technical guide | `["Linux"]`, `["Arch Linux"]` etc. | Problem → Prerequisites → Step-by-step instructions → Verification |
| Opinion/reflection | `["thoughts"]` | Observation → Deeper thinking → Personal takeaway → Open question |
| Experiment/debugging | `["Experiment", ...]` | What went wrong → Investigation → Root cause → Solution |

---

## Image & Asset Conventions

- Store images in `assets/images/{post-slug}/` (one folder per post, slug must match post filename).
- Cover banner: name it `banner.png` inside the slug folder.
- Reference in frontmatter: `images/{slug}/banner.png`
- Reference in markdown body: `![Alt](images/{slug}/filename.png)`
- Supported formats: `.png`, `.webp`, `.jpg`, `.jpeg`, `.gif`
- Images are processed through Hugo's asset pipeline (not raw `static/` files).

### Image Checklist for New Post

- [ ] Create `assets/images/{slug}/` directory
- [ ] Add `banner.png` (cover image)
- [ ] Add all screenshots/photos to that folder
- [ ] Update `cover.image` in frontmatter
- [ ] Inline images use paths relative to `assets/`: `![Alt](images/{slug}/filename.png)`

---

## Tags System

All categorization uses `tags` – there is no separate `categories` field.

| Tag Convention | Purpose |
|---------------|---------|
| `"Projects"` | Triggers homepage carousel feature + menu link |
| `"AI"`, `"LLM"`, `"Machine Learning"` | AI-related posts |
| `"thoughts"` | Reflective, non-technical opinion pieces |
| `"Linux"`, `"Arch Linux"` | Technical guides |
| `"Experiment"` | Debugging/investigation posts |
| Specific project names | `"Traveed"`, `"Smeltub"`, `"Qiai"` etc. |

Tags generate listing pages at `/tags/{tagname}/` (or `/en/tags/{tagname}/`).

### Tag Rules

- Use 2-6 tags per post.
- Always include category-level tags plus specific tags.
- Project posts: always include `"Projects"` as the first tag, plus technology-specific tags.
- Both language files must have identical tags.

---

## Quick Reference: New Blog Post Checklist

1. [ ] Pick a kebab-case slug (e.g. `My-New-Project`)
2. [ ] Run `hugo new --kind post content/posts/{slug}.md`
3. [ ] Edit the `.md` file: set `title`, `description`, `tags`, real `date`, `draft: false`
4. [ ] Write the Indonesian body following the style guide above
5. [ ] Copy `.md` → `.en.md` and translate naturally
6. [ ] Fix `editPost.URL` in `.en.md` to point to the `.md` file
7. [ ] Create `assets/images/{slug}/` and add images
8. [ ] Set `cover.image` in frontmatter (if applicable)
9. [ ] Verify `date` matches, `tags` match, slug matches across both files
10. [ ] Run `hugo server` and test both `/posts/{slug}/` and `/en/posts/{slug}/`

---

## Commands

```bash
# Create a new post from archetype
hugo new --kind post content/posts/My-Post-Slug.md

# Start dev server (drafts included)
hugo server -D

# Build for production
hugo

# Build minified for production
hugo --minify
```
