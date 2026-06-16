---
title: "Lazy Teacher"
date: 2025-03-24T00:00:00+07:00
tags: ["Projects", "AI", "LLM", "langchain", "library"]
author: "Me"
showToc: true
TocOpen: false
draft: false
hidemeta: false
comments: false
description: "Library that allows for automatic question generation and evaluation using the help of llm, built on langchain."
searchHidden: false
ShowReadingTime: true
ShowBreadCrumbs: true
ShowPostNavLinks: true
ShowWordCount: true
ShowRssButtonInSectionTermList: true
UseHugoToc: true
cover:
    image: "images/lazyteacher/banner.png" # image path/url
    alt: "Lazy Teacher" # alt text
    caption: "Lazy Teacher" # display caption under cover
editPost:
    URL: "https://github.com/ripabak/ripabak.github.io/content/posts/Lazy-teacher.md"
    Text: "Suggest Changes"
    appendFilePath: true
---


Some time ago, I was working on an e-learning related project. Simply put, teachers can create questions and students can answer them there.

I made the system like Google Forms. After the teacher creates the questions, there will be a link that students can access to answer the questions.

During the creation process, I thought, why don't I add AI to this system? I know... not all software needs AI, it depends on the needs. Here I will discuss the creation process, from ideation to implementation.

But here I felt that instead of the teacher having to check each student's answer one by one, why couldn't it happen automatically? The teacher's POV would only focus on making questions and providing the correct answers, the rest would be handled by AI acting as a judge between the correct answer and the student's answer.

This is the basis for why I created Lazy Teacher. I made it separate with the hope that it can be integrated with existing e-learning systems.

I tried to do some research... by utilizing LLM (btw this was also hype when this was written), what features could I add? Some conclusions appeared.
- LLM can act as a judge, so teachers no longer need to check student answers one by one.
- LLM can create questions automatically.

Before I started making Lazy Teacher, I needed to do a little research. First, what kind of questions would there be, when giving grades to students usually what range do teachers give, is there tolerance if, for example, a student answers with a typo?

From these questions, I created several features in Lazy Teacher.
- There are several types of questions that can be made, I took the general ones here `ShortAnswer`, `Essay`, or `MultipleChoice`.
- Can create questions manually.
- Can create questions automatically, either from documents or simple prompts, and can choose the number of questions too.
- Can evaluate student answers automatically.
- The value of each question can be set.

Some examples of its use.
- Generate questions from prompt
```typescript
import { QuestionGenerator } from 'lazyteacher/generator';

const questionGenerator = new QuestionGenerator();
const user_prompt = "Question about AI Agent for future";
const total_questions = 3;
const question_type = "multiple_choice";

const generated_questions = await questionGenerator.generateFromPrompt({ question_type: question_type, user_prompt: user_prompt, total_questions: total_questions });

console.dir(generated_questions);
```
We can set several things here, for example, the total questions, question type, and also the prompt.

- Generate questions from document
```typescript
import { QuestionGenerator } from 'lazyteacher/generator';
import * as fs from "fs";

const total_questions = 7;
const content = fs.readFileSync("example.txt", "utf-8");

const questionGenerator = new QuestionGenerator();

const question_generated = await questionGenerator.generateFromDocument({
    question_type: "short_answer",
    document_text: content, // in string
    total_questions: total_questions,
    chunk_size: 1000,
    chunk_overlap: 50,
    document_query: "Jakarta", // query relevant document (optional)
    top_query: 5 // top query relevant (optional)
});
```
Here the concept is still the same, but there are some additional parameters such as chunk size and chunk overlap to regulate how the document is broken down, as well as document query and top query to determine which parts of the document are most relevant to take. This is actually utilizing features already provided by LangChain, and it is very helpful so that the context given to the LLM can be more controlled.

Example output:
```json
[
    MultipleChoice {
        question: 'What is one potential benefit of AI agents mentioned in the text?',
        correct_answer: 0,
        type: 'multiple_choice',
        choices: [
        'Enhancing productivity',
        'Increasing job displacement',
        'Reducing data privacy',
        'Limiting personalized experiences'
        ],
        answer: undefined,
        score: undefined
    },
    MultipleChoice {
        question: 'What is a significant challenge associated with the rise of AI agents?',
        correct_answer: 1,
        type: 'multiple_choice',
        choices: [
        'Streamlining operations',
        'Data privacy concerns',
        'Improving healthcare',
        'Enhancing customer service'
        ],
        answer: undefined,
        score: undefined
    },
    MultipleChoice {
        question: 'According to the text, what is crucial as we integrate AI agents into our lives?',
        correct_answer: 2,
        type: 'multiple_choice',
        choices: [
        'Maximizing job displacement',
        'Ignoring ethical considerations',
        'Striking a balance between benefits and societal implications',
        'Focusing solely on technological advancements'
        ],
        answer: undefined,
        score: undefined
    }
]
```

There are still many more features that can be explored, such as related to evaluation, scores, etc. For more details, I suggest you go directly to the documentation on github, https://github.com/ripabak/LazyTeacher. Thank you for reading :)
