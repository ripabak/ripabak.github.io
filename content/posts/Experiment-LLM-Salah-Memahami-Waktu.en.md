---
title: "Experiment: LLM Misunderstanding Time"
date: 2026-02-12T23:00:00+07:00
# weight: 1
# aliases: ["/first"]
tags: ["Experiment", "LLM", "Traveed"]
author: "Me"
# author: ["Me", "You"] # multiple authors
showToc: true
TocOpen: false
draft: false
hidemeta: false
comments: false
description: "Experiment with LLM misunderstanding time and solutions applied."
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
editPost:
    URL: "https://github.com/ripabak/ripabak.github.io/content/posts/Experiment-LLM-Salah-Memahami-Waktu.md"
    Text: "Suggest Changes" # edit text
    appendFilePath: true # to append file path to Edit link
---

Some time ago, I felt specifically that the AI workflow in my app was not working as expected. This happened several times even with the same workflow I had done previously. I didn't change anything, so my assumption is that the capabilities of the OpenAI `gpt-4.1-nano` model have changed.

What happened to me was that the output from the `gpt-4.1-nano` LLM model often produced results that did not meet expectations. It gave the wrong time output, which is a major concern specifically for my application. Therefore, for the time being, I moved to `gpt-5-nano` because it is still relatively cheap, *it worked*, it is smarter, but a new problem appeared.

`GPT-5-nano` takes a long time and requires huge tokens (can be 4x larger) due to *reasoning*, whereas my system does not require it and also because I use my own internal *reasoning* system which is simpler and has small token usage.

I tried looking for a way to disable it, but it turns out some models do not support disabling *reasoning*, such as `gpt-5-nano` itself. I considered `gpt-5-nano` because the price is also cheaper compared to the previous nano model (`gpt-4.1-nano`), but the problem here is that the response takes a very long time due to *reasoning*.

Only a few models support disabling *reasoning* for GPT-5 versions and above, you can check here:
- [Need reasoning false option for GPT-5 update](https://community.openai.com/t/need-reasoning-false-option-for-gpt-5-update-gpt-5-1-solves-reasoning-issue/1351588)

I tried to compare the price and specification differences on the OpenAI models detail page. Here are the differences for the nano models, GPT-4.1 nano and GPT-5 nano.

![gpt.4.1.nano](images/experiment-llm-salah-memahami-waktu/GPT.4.1.nano.png)
![gpt.5.nano](images/experiment-llm-salah-memahami-waktu/GPT.5.nano.png)

<!-- Image Placeholder: Price and specification differences of nano models -->

When this article was written, GPT-5 nano was cheaper in terms of INPUT-OUTPUT price but its *reasoning* cannot be disabled. If you think about it, it doesn't end up being cheap either because the tokens are larger and it also takes a long time for a response.

So I want to try to stay on `gpt-4.1-nano` so the price remains cheap and fast. The problem here is how I can overcome that issue. This article will discuss the use case that occurred and the experiments I conducted.

## Requirements and Approach

My requirement is that I need the model to understand the timing from the user text input, such as "this morning", "2 days ago", etc. The approach I have taken so far is to take the *current time* from the server and provide it into the context and set the prompt template to follow that. I did that because LLM models by default do not know the current time accurately as the model only knows information from the training data.

This worked as expected in many cases, but recently it hasn't, and I tried to find out the problem and I discuss it here.

The AI flow of my system is roughly like this:

![AI-flow](images/experiment-llm-salah-memahami-waktu/AI-flow.png)

You might ask why generating user intent is necessary? The answer is because if the output directly extracts structured output to JSON, in many cases the result is hallucination. That's why I use this approach to first understand the user's intent in a structured way, then I extract it into JSON with the information that is actually needed. This could be called a simple *Chain-of-thought* for this matter.

For the COT prompt template, I ask the model to be like this:

```text
…

follow this response format:
Wait, I need to focus & understand carefully when the expense should be added to add expenses. I think user wants to add (fill in the blank) from (indicate the time range), current time is (HH:mm DD/MM/YYYY). therefore the expense list is:
- 12,000 IDR for breakfast in the morning at 08:00 29/12/2023
- 16,000 IDR for lunch at an angkringan with chicken at 12:00 30/12/2023

…
```

## Problems Found

Straight to the point, here are some examples of AI responses that did not meet expectations.

**User Input:** `i bought coffe 15000 rupiah`

**Generated User Intent:**
> Wait, I need to focus & understand carefully when the expense should be added to add expenses. I think user wants to add a coffee expense of 15,000 IDR today. therefore the expense list is:
> - 15,000 IDR for coffee at 20:10 28/12/2023

**AI Final:**
```json
{"expenses": [{"name": "Coffee", "amount": 15000, "category": "Food & Drink", "currency": "IDR", "description": "", "transactionDate": "2023-12-28T13:10:00.000Z"}]}
```

**User Input:** `Makan malam 15rb beli ayam murotal`

**Generated User Intent:**
> Wait, I need to focus & understand carefully when the expense should be added to add expenses. I think user wants to add a dinner expense of 15,000 IDR for buying Murotal chicken, and the transaction probably happened in the evening today. therefore the expense list is:
> - 15,000 IDR for dinner with Murotal chicken at 19:00 28/11/2023

**AI Final:**
```json
{"expenses": [{"name": "Dinner with Murotal Chicken", "amount": 15000, "category": "Food & Beverage", "currency": "IDR", "description": "Bought at Murotal Chicken restaurant", "transactionDate": "2023-11-28T12:00:00.000Z"}]}
```

It can be seen that the AI response was not prompted to mention the *current time* in its user intent, I think this is because my instructions were not strict enough.

Eh, turns out there is a log that mentions *current time* but it is wrong.

**User Input:** `Lunch for 60 twd`

**Generated User Intent:**
> Wait, I need to focus & understand carefully when the expense should be added to add expenses. I think user wants to add a lunch expense of 60 TWD from an unspecified time, current time is 12:52 31/12/2023. therefore the expense list is:
> - 60 TWD for lunch at the current time 12:52 31/12/2023

**AI Final:**
```json
{"expenses": [{"name": "Lunch", "amount": 60, "category": "Food", "currency": "TWD", "description": "", "transactionDate": "2023-12-31T04:52:00.000Z"}]}
```
**createdAt:** `2025-12-31 04:52:17.271`

“current time is 12:52 31/12/2023” whatt? how? I actually included the current time date as context in the system prompt at the beginning, is the server time different? hmm maybe.

My assumptions here are 2:
1. The server *current time* is wrong.
2. The LLM is hallucinating, because in my example instructions I mentioned 2023.

I tried to look again at the latest user intent log data:

> "Wait, I need to focus & understand carefully when the expense should be added to add expenses. I think user wants to add a purchase for mineral and snacks for 14,000 IDR, but the time of purchase is not specified. Therefore, the expense list is:
> - 14,000 IDR for mineral and snacks at (current time) (current date)"

> "Wait, I need to focus & understand carefully when the expense should be added to add expenses. I think user wants to add a transaction for Warteg with a cost of 10,000 IDR. Therefore, the expense list is:
> - 10,000 IDR for Warteg at (current time in HH:mm DD/MM/YYYY)"

> "Wait, I need to focus & understand carefully when the expense should be added to add expenses. I think user wants to add a keychain purchased for 90 TWD, but no specific time mentioned. Therefore, the expense list is:
> - 90 TWD for keychain at current time 14:33 23/01/26"

> "Wait, I need to focus & understand carefully when the expense should be added to add expenses. I think user wants to add expenses related to dinner, train, bubble tea, and biscuits, all in TWD, but no specific time range or current time specified. Therefore the expense list is:
> - 170 TWD for dinner at current time
> - 44 TWD for train at current time
> - 80 TWD for bubble tea at current time
> - 25 TWD for biscuits at current time"

Wait.. what, it didn't generate the current time part correctly... hmm.
- LLM wrote it with the wrong format
- LLM rewrote the instructions

But.. the 'keychain' part actually has the correct current time in the user intent, but ... the Final Output is still wrong apparently, here is the log data:

**User Input:** `Keychain for 90 twd`

**Generated User Intent:**
> Wait, I need to focus & understand carefully when the expense should be added to add expenses. I think user wants to add a keychain purchased for 90 TWD, but no specific time mentioned. Therefore, the expense list is:
> - 90 TWD for keychain at current time 14:33 23/01/26

**AI Final:**
```json
{"expenses": [{"name": "keychain", "amount": 90, "category": "Shopping", "currency": "TWD", "description": "a keychain purchased", "transactionDate": "2023-01-26T06:33:00.000Z"}]}
```
**createdAt:** `2026-01-23 06:33:15.640`

Another new problem hmm...

Look, apparently the LLM is confused, “23/01/26” should be the 23rd of the year 2026, not the 26th of the year 2023, oh my god it's hard dealing with generated text like this lol.

## Problem Analysis

My assumption points to the LLM...
Can luxon produce null so that the LLM doesn't get the current time now? I tried consulting ChatGPT about the 'luxon' library, it said:

<!-- Image Placeholder: Explanation of Luxon from ChatGPT -->

Okay I take that as a fact.. if the DateTime is invalid, maybe the assumption that the current time results in 2023 is because the server could be correct too, getting more confused...

Okay fine, my assumption now is that both have the potential to be the problem:
1. The current time is wrong, maybe because of the server, I use Vercel, maybe there is a connection because of serverless cold restart.
2. The LLM is wrong, this is probably because I was also less strict in terms of prompting.

I didn't verify if the server time was different, it's quite complicated to do because of that.

## Applied Solutions

The solution I tried to apply:
1. Force the client to send their current time, this is less likely to change I feel regarding the current time, unless the user changes the time & date on their device.
2. Improve the AI prompt template, to force it to follow the instruction for the user intent response form, I added this as one of them.

![prompt-improvement](images/experiment-llm-salah-memahami-waktu/prompt-improvement.png)

Is the problem solved? **NOOO**, I'm happy found another problem, look at this log:

**User Input:** `Lunch for 60 twd`

**Generated User Intent:**
> Wait, I need to focus & understand carefully when the expense should be added to add expenses. I think user wants to add a lunch expense of 60 TWD, but I need to know the specific time when this happened. current time is 16:08 12/02/2026. therefore the expense list is:
> - 60 TWD for lunch at (time not specified) (12/02/2026)

**Final AI:**
```json
{"expenses": [{"name": "Lunch", "amount": 60, "category": "Food & Drink", "currency": "TWD", "description": "", "transactionDate": "2026-12-01T17:00:00.000Z"}]}
```

Again the LLM misunderstood the date format, it should be the 12th of month 2, the LLM was confused and assumed that the transactionDate was the 2nd of month 12, so the transactionDate was wrong.. it's okay.. so now the challenge is how to make the LLM understand a consistent date format. I changed the system prompt side:

My system prompt template had not adapted to ISO Format yet, so I changed it to be like this. To be more universal, it could be UTC+0, UTC+7, etc.

![improve-response](images/experiment-llm-salah-memahami-waktu/improve-response.png)

It turns out there is another indication why it was 2023, which is from my own prompt giving an example of 2023, maybe it could be biased towards that so the LLM hallucinated.

Now this can be more consistent, I tried with user input = “Lunch for 60 twd”:

**Generated User Intent:**
> Wait, I need to focus & understand carefully when the expense should be added to add expenses. I think user wants to add a lunch expense of 60 TWD today, current time is 2026-02-12T16:22:25.638+07:00. Therefore the expense list is:
> - 60 TWD for lunch at date 2026-02-12T16:22:25.638+07:00

**AI Final:**
```json
{"expenses": [{"name": "Lunch", "amount": 60, "category": "Food & Drink", "currency": "TWD", "description": "Lunch expense", "transactionDate": "2026-02-12T09:22:25.638Z"}]}
```

**Generated User Intent:**
> Wait, I need to focus & understand carefully when the expense should be added to add expenses. I think user wants to add a lunch expense of 60 TWD. Since no specific date is mentioned, I will assume it was made today. Therefore the expense list is:
> - 60 TWD for lunch at date 2026-02-12T16:21:30.272+07:00

**AI Final:**
```json
{"expenses": [{"name": "Lunch", "amount": 60, "category": "Food & Drink", "currency": "TWD", "description": "", "transactionDate": "2026-02-12T09:21:30.272Z"}]}
```

## Conclusion

At least now, the output is more consistent for the transaction date issue, although it can be seen that there are generated user intent results where the current time sentence does not appear :(. At least now the results are not hallucinations like before. This will continue to be improved and if I find something interesting, I will share it on my personal blog [ripabak.github.io](https://ripabak.github.io).

The conclusion is, when playing with LLM for strict predicted output cases like this, the system prompt must also be strict, especially if using a cheap and fast model. Every problem related to LLM hallucination must be strict in terms of prompting. Hope it helps.
