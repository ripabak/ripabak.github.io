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


Beberapa waktu yang lalu, aku lagi ngerjain project terkait e-learning, sederhana nya nantinya guru bisa membuat soal dan siswa bisa menjawab soal dari sana. 

Aku buat sistem nya seperti Google Form, setelah guru membuat soal akan ada link yang bisa di akses oleh siswa untuk menjawab soal.

Pada proses pembuatan aku terpikirkan kenapa tidak aku menambahkan AI di sistem ini, aku tahu... tidak semua software harus ada AI nya ya, balik lagi sesuai kebutuhan. Nah disini akan aku bahas proses pembuatan nya, mulai dari ideation sampai implementasi.

Tapi di sini aku merasa daripada guru harus periksa satu-satu jawaban dari siswa nya, kenapa tidak itu bisa terjadi secara otomatis saja (?). POV guru hanya fokus ke membuat soal dan memberikan jawaban yang benar, sisa nya akan ada AI yang akan bertindak sebagai judger dari jawaban yang benar dan jawaban siswa.

Ini yang mendasari kenapa aku membuat Lazy Teacher. Aku membuat nya terpisah dengan harapan bisa di integrasi dengan sistem e-learning yang sudah ada.

Aku coba riset... dengan memanfaatkan LLM (btw ini juga lagi hype juga ketika tulisan ini ditulis), apa saja kira-kira fitur yang bisa aku tambahkan (?). Muncul beberapa kesimpulan.
- LLM bisa bertindak sebagai judger, jadi guru tidak perlu lagi periksa satu-satu jawaban siswa.
- LLM bisa membuat soal secara otomatis.

Sebelum aku mulai membuat Lazy Teacher, aku perlu membuat riset sedikit, yang pertama adalah jenis soalnya bakalan apa saja, ketika memberi nilai ke siswa biasanya guru memberi rentang berapa, apakah ada toleransi jika semisal siswa menjawab nya typo (?).

Dari pertanyaan ini aku membuat beberapa fitur di Lazy Teacher.
- Ada beberapa jenis soal yang bisa di buat, aku di sini ambil secara umum `ShortAnswer`, `Essay`, or `MultipleChoice`.
- Bisa membuat soal secara manual.
- Bisa membuat soal secara otomatis, bisa dari dokumen atau prompt sederhana, bisa pilih berapa jumlah soalnya juga.
- Bisa mengevaluasi jawaban siswa secara otomatis.
- Nilai masing-masing soal bisa di atur.

Beberapa contoh penggunaan nya.
- Generate soal dari prompt
```typescript
import { QuestionGenerator } from 'lazyteacher/generator';

const questionGenerator = new QuestionGenerator();
const user_prompt = "Question about AI Agent for future";
const total_questions = 3;
const question_type = "multiple_choice";

const generated_questions = await questionGenerator.generateFromPrompt({ question_type: question_type, user_prompt: user_prompt, total_questions: total_questions });

console.dir(generated_questions);
```
Kita bisa atur beberapa hal disini, misalnya total soal, jenis soal, dan juga prompt nya.

- Generate soal dari dokumen
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
Di sini konsepnya masih sama, tapi ada beberapa parameter tambahan seperti chunk size dan chunk overlap untuk mengatur bagaimana dokumen dipecah, serta document query dan top query untuk menentukan bagian dokumen mana yang paling relevan untuk diambil. Ini sebenarnya pemanfaatan fitur yang sudah disediakan oleh LangChain, dan sangat membantu supaya konteks yang diberikan ke LLM bisa lebih terkontrol.

Contoh output nya akan seperti ini :
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

Masih ada banyak lagi fitur-fitur nya yang bisa di eksplor, seperti terkait evaluasi, score, dll. untuk lebih lengkap nya, aku menyarankan kamu untuk langsung ke dokumentasi nya di github, https://github.com/ripabak/LazyTeacher. Thank you for reading :)