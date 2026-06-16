---
title: "Traveed"
date: 2026-02-09T12:30:30+07:00
# weight: 1
# aliases: ["/first"]
tags: ["Projects", "AI", "LLM", "Travel", "Expense Tracker", "Budgeting"]
author: "Me"
# author: ["Me", "You"] # multiple authors
showToc: true
TocOpen: false
draft: false
hidemeta: false
comments: false
description: "AI-powered expense tracking app that I use daily"
disableHLJS: true # to disable highlightjs
disableShare: false
hideSummary: false
searchHidden: false
ShowReadingTime: true
ShowBreadCrumbs: true
ShowPostNavLinks: true
ShowWordCount: true
ShowRssButtonInSectionTermList: true
UseHugoToc: true
cover:
    image: "images/traveed/banner.png" # image path/url
    alt: "Traveed" # alt text
    caption: "Traveed" # display caption under cover
    relative: false # when using page bundles set this to true
    hidden: false # only hide on current single page
editPost:
    URL: "https://github.com/ripabak/ripabak.github.io/content/posts/Traveed.en.md"
    Text: "Suggest Changes" # edit text
    appendFilePath: true # to append file path to Edit link
---

There are many expense tracking apps out there, but my main concern now is how to make expense tracking less manual—no need to fill in lots of data like item name, price, category, and so on.

But before that, there's an experience that became the reason why expense tracking is necessary: when I was abroad. I live in Indonesia, where the cost of living is relatively cheaper compared to other countries. The currency difference made it difficult for me to compare how much I had spent so far.

At the time of writing this article, people often use ChatGPT for almost everything. This has become a habit because of the convenience and efficiency it provides.

I thought, why not use a similar flow for expense tracking? So users would just need to input what they bought, the price, where they bought it, when they bought it, and so on.

With the current development of LLMs, I believe this is possible, and that became the foundation for why I created Traveed.

## Features

Some of the major features of Traveed include:

- **AI Expense Capture**: Users only need to enter expenses by typing in everyday language, for example "Bought noodles for 15000 rupiah at Mrs. Ani's stall", and the AI will automatically separate that input into name, description, amount, and currency.

<div style="display: flex; gap: 10px; justify-content: center;">
  <img src="/images/traveed/transactions.png" alt="Transactions" style="width: 32%;" />
  <img src="/images/traveed/analytics.png" alt="Analytics" style="width: 32%;" />
</div>

- **Prompt Tuning**: This is useful for customizing how the AI enters expense data. For example, if users want the AI to enter expense data in a specific format, language, or even style, they can set up prompt tuning to configure that format.

![Prompt Tuning](/images/traveed/ai-personalized.png)

- **160+ Currencies Supported**: Supports various currencies including IDR, USD, JPY, and others, with automatic conversion back to the main currency. This makes it easy for users to compare their expenses across different countries.

![Currencies](/images/traveed/currencies.png)

- **Custom Categories**: Users can set up their expense categories, and the AI automatically understands the available categories and uses them to categorize expenses.

![Categories](/images/traveed/categories.png)

## Summary
<iframe src="https://traveed.app" width="100%" height="600" style="border:none; border-radius: 12px; box-shadow: 0 4px 20px rgba(0,0,0,0.1); margin: 20px 0;"></iframe>

As a developer, I use Traveed as my daily expense tracking app, and so far I'm satisfied with the existing features and continue to make improvements. It's currently in BETA and can be accessed for free at [https://traveed.app](https://traveed.app).
