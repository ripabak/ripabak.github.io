---
title: "ChatYourPage, embed agent to your website"
date: 2026-08-11T12:00:00+07:00
tags: ["Projects", "AI", "LLM", "Agent", "RAG"]
author: "Me"
showToc: true
TocOpen: false
draft: false
hidemeta: false
comments: true
description: "A RAG-powered AI Agent you embed with one line of code"
showReadingTime: true
ShowBreadCrumbs: true
ShowPostNavLinks: true
ShowWordCount: true
ShowRssButtonInSectionTermList: true
UseHugoToc: true
cover:
    image: "images/chatyourpage/banner.png"
    alt: "ChatYourPage landing page"
    caption: "ChatYourPage — embed an AI agent on your website with one line of code"
    hiddenInSingle: true
editPost:
    URL: "https://github.com/ripabak/ripabak.github.io/content/posts/ChatYourPage.md"
    Text: "Suggest Changes"
    appendFilePath: true
---

A few days ago I was chatting with a friend who showed off his portfolio website. It was just a simple chatbot, honestly — one that answers questions based on who he is. *Not really impressed* at first.

What actually impressed me was something simple he mentioned: **a decent number of people had tried his chatbot, and he could tell from the tokens used**. That sparked an idea — why don't I build my own? I wanted to know how many people use my chatbot, and I'd get to learn the implementation along the way.

That's how [ChatYourPage](https://chatyourpage.com/) was born.

Here's a quick demo:

{{< video src="videos/chatyourpage-demo-com.mp4" >}}

## More than just a Q&A chatbot

Unlike a chatbot that can only answer questions, I turned this project into an **AI Agent** again. *Overkill?* Maybe. But I think the use case justifies it.

The chatbot isn't just Q&A anymore — it's **connected to several tools**, one of them being `RAG` (*Retrieval Augmented Generation*), which lets the agent answer from a large database. You just feed it the info: your products, services, opening hours, rules, `FAQ`, or whatever you want.

### How RAG works, simply

Under the hood, documents are split into *chunks* and stored as *vectors* using an **embedding model**. The same embedding model is then used again to *search* for the relevant information — that's basically it.

This `RAG` approach is super useful because **the agent can search the document itself instead of shoving everything into the model** — which also makes token usage much more efficient, since not all information is fed to the model directly.

## Flexible knowledge base

I built [ChatYourPage](https://chatyourpage.com/) to support several document types:

- `PDF` — upload files directly
- `URL` — just paste the page address
- Long *plain text* — copy-paste docs, notes, policies

So it's flexible enough to grow your *knowledge base* later.

Btw, **the website itself uses it too** — you'll see a floating button you can click (usually at the bottom right). Ask it anything.

## The agent can also explore pages

The ChatYourPage agent isn't just `RAG` — it also **knows the current page's content** and can *fetch* specific pages for deeper context.

It works great on *landing pages*. Say your `FAQ` page is separate — you can ask the agent about the `FAQ` and it will explore, fetching the page if the info isn't available on the main page. **It's flexible enough to decide internally what to do.**

## Configurable to make it stand out

There are a few things you can configure so the chatbot feels right for your brand:

- **Agent personalization** — pick an avatar template
- **Widget color personalization** — just choose a theme for the chat panel
- Finally, you can **see token usage**, the number of requests, and where those requests came from — a neat way to get a sense of your visitors :)

![Customize your ChatYourPage agent](images/chatyourpage/customize-chatyourpage_opt.png)

## Go try it!

You can try it right now — at the time of writing it's still **BETA and free**, so go ahead and install it on your own website at [chatyourpage.com](https://chatyourpage.com/). Thanks for reading, hope it helps :)
