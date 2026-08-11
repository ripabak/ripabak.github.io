---
title: "CV-Tailor, Building Your Resume with an AI Agent"
date: 2026-08-10T12:00:00+07:00
tags: ["Projects", "AI", "LLM", "Agent", "HTML"]
author: "Me"
showToc: true
TocOpen: false
draft: false
hidemeta: false
comments: true
description: "An AI Agent that creates and edits your resume through chat — just copy-paste your old resume and let the AI edit the code directly."
showReadingTime: true
ShowBreadCrumbs: true
ShowPostNavLinks: true
ShowWordCount: true
ShowRssButtonInSectionTermList: true
UseHugoToc: true
cover:
    image: "images/cv-tailor/banner.png"
    alt: "CV-Tailor demo"
    caption: "CV-Tailor demo"
    hiddenInSingle: true
editPost:
    URL: "https://github.com/ripabak/ripabak.github.io/content/posts/Cv-Tailor.md"
    Text: "Suggest Changes"
    appendFilePath: true
---

Before we go further, here's a quick demo:

{{< video src="videos/cv-tailor-demo.mp4" >}}

If you're curious, try it right away at **[cv-tailor.fire.my.id](https://cv-tailor.fire.my.id/)** 👇


One of the articles that inspired me is [Create Your Resume in HTML and CSS](https://wkaisertexas.github.io/blog/create-your-resume-in-html-and-css/), which talks about why he uses `HTML` and `CSS` to build his resume. As he said, most programmers make their resumes in `LaTeX` — and **"YES", I'm one of those**.

I could use tools like [Overleaf](https://www.overleaf.com/) or set everything up in my own IDE, but I realized my approach **takes time** and isn't very *user friendly*. Not everyone can just try it right away — you need setup beforehand if you're working on a local machine. And in this era of AI, users like me expect to be **effortless** when making their resumes.

> "Almost everything can be done by AI nowadays"

And I believe **an LLM can do that for me** — that's what made me build this tool, **"CV-Tailor"**.

I'm leveraging the concept of **AI Agents**. Most platforms we use every day — `ChatGPT`, `Claude`, `Gemini` — rely on this concept to make their AI more capable.

Simply put, *agentic* combines the power of an `LLM` with **tools it can access**. There are so many ideas that can come out of this simple concept.

You can see it in how `Gemini` searches the internet for information in real time. An `LLM` can't do that *by default*, which is why a **search tool** is given to the `LLM` — so it can access the web and produce *up to date* results.

## How does the AI Agent concept work in CV-Tailor?

Well, one of the challenges of building an AI Agent for CV-Tailor was figuring out **what tools to create** that it could actually use.

After researching various coding editor tools out there — `OpenCode` being one of them — we need to give it the ability to edit the `HTML` code. The tools also had to be *designed* so they're easy for the `LLM` to use directly, just by filling in the parameters.

### The 5 Core Tools

There are 5 main tools built so the `LLM` can edit your resume right away:

- `get_current_html()` — gets the full `html` text, along with its *line numbers*, for the `edit_lines(start, end, new_content)` tool.
- `edit_lines(start, end, new_content)` — directly edits specific lines, replacing them with the latest `html` content.
- `read_lines(start, end)` — reads specific lines. This was useful for making sure the lines you wanted to edit matched before running `edit_lines()`. **LLMs are still bad at guessing line numbers from code**, so `read_lines()` would run over and over until it reached the right section. But since `get_current_html()` was *updated* to include line numbers directly, this tool ended up being abandoned.
- `cv_replace(old, new)` — another editing tool, an alternative to `edit_lines()`, but with a different approach. Once the `LLM` knows which code it wants to replace, it can call this tool directly with the old code and the new code.
- `cv_replace_all(old, new)` — same as `cv_replace()`, but it replaces every occurrence of the old content, like a *find & replace all* feature. Great for **keeping things consistent when names change**, certain terms, etc. Though it's rarely used.

There are actually many more tools being used in CV-Tailor, like for *long term memories* and features that make the platform *user friendly*. **It'll keep evolving as needs grow.**

Go ahead and try it — **copy all the content from your old resume and paste it right into the chat!** Let AI do the rest for ya :)
