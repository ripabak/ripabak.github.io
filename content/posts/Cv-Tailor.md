---
title: "CV-Tailor, bikin resume pakai AI Agent"
date: 2026-08-10T12:00:00+07:00
tags: ["Projects", "AI", "LLM", "Agent", "HTML"]
author: "Me"
showToc: true
TocOpen: false
draft: false
hidemeta: false
comments: false
description: "AI Agent yang mempermudah aku memperbagus resume secara mudah dan otomatis"
showReadingTime: true
ShowBreadCrumbs: true
ShowPostNavLinks: true
ShowWordCount: true
ShowRssButtonInSectionTermList: true
UseHugoToc: true
cover:
    image: "images/cv-tailor/banner.png"
    alt: "Demo CV-Tailor"
    caption: "Demo CV-Tailor"
    hiddenInSingle: true
editPost:
    URL: "https://github.com/ripabak/ripabak.github.io/content/posts/Cv-Tailor.md"
    Text: "Suggest Changes"
    appendFilePath: true
---

Sebelum lanjut, nih demo singkatnya:

{{< video src="videos/cv-tailor-demo.mp4" >}}

Kalau penasaran, langsung cobain di **[cv-tailor.fire.my.id](https://cv-tailor.fire.my.id/)** 👇


Salah satu artikel yang membuatku terinspirasi adalah [Create Your Resume in HTML and CSS](https://wkaisertexas.github.io/blog/create-your-resume-in-html-and-css/), ketika membahas tentang alasan kenapa dia menggunakan `HTML` `CSS` untuk membuat resume. Seperti katanya, kebanyakan programmer akan membuat resume mereka dalam format `LaTex`, dan **"YES", I'm one of those**.

Aku bisa pakai tool seperti [Overleaf](https://www.overleaf.com/) atau setup di IDE ku sendiri, aku sadar caraku tersebut **takes time** dan kurang *user friendly*, tidak semua orang bisa langsung mencobanya, perlu setup di awal kalau menggunakan local machine, dan juga di zaman AI seperti sekarang ini, user seperti aku akan berharap bisa **effortless** ketika membuat resume-nya.

> "Almost everything can be done by AI nowadays"

Dan aku percaya **LLM can do that for me**, itu yang membuat aku membuat tools ini, **"CV-Tailor"**.

Aku memanfaatkan konsep dari **AI Agent**, kebanyakan platform menggunakan ini — `ChatGPT`, `Claude`, `Gemini` yang biasa kita pakai — mereka menggunakan konsep ini agar AI mereka bisa menjadi lebih canggih.

Simpelnya, *agentic* menggabungkan kemampuan `LLM` dengan **tools yang bisa diakses olehnya**. Ada banyak ide-ide yang bisa muncul dari konsep yang sederhana ini.

Sebagai contoh, `Gemini` bisa dilihat dapat mencari informasi dari internet secara langsung. `LLM` tidak bisa melakukan itu *by default*, makanya **tool pencarian** diberikan ke `LLM` agar bisa mengaksesnya, dan informasi yang dihasilkan akan *up to date*.

## Bagaimana konsep AI Agent di CV-Tailor?

Well, salah satu challenging ketika membuat AI Agent untuk CV Tailor adalah **tools apa yang sebaiknya dibuat** dan bisa dipakai olehnya.

Setelah melakukan riset dari berbagai tools coding editor di luar sana seperti `OpenCode` salah satunya, kita perlu memberikan kemampuan untuk mengedit `HTML` code-nya. Tools yang dibuat perlu di-*design* juga agar cocok dan bisa digunakan langsung oleh `LLM` dengan hanya mengisi parameter pada tools-nya.

### 5 Tools Utama

Ada 5 tools utama yang dibuat agar `LLM` bisa langsung melakukan edit resume:

- `get_current_html()` — untuk mendapatkan full `html` teksnya, beserta *number lines*-nya untuk tool `edit_lines(start, end, new_content)`.
- `edit_lines(start, end, new_content)` — edit langsung baris tertentu, ia akan melakukan *replace* dengan konten `html` terbaru.
- `read_lines(start, end)` — untuk baca baris tertentu, ini sebelumnya berguna untuk memastikan baris yang ingin diedit telah sesuai atau belum sebelum menjalankan `edit_lines()`. **LLM masih jelek untuk menebak nomor baris dari kode**, `read_lines()` akan dijalankan berkali-kali sampai mendapatkan ke bagian yang diinginkan, tapi karena tool `get_current_html()` sudah di-*update* agar bisa langsung memberikan nomor dari barisnya, jadi tool ini akan ditinggalkan.
- `cv_replace(old, new)` — ini tool edit juga, alternatif dari `edit_lines()`, cara kerjanya berbeda. Setelah `LLM` tahu kode yang ingin diganti, `LLM` bisa langsung memanggil tool ini dengan memasukkan kode yang ingin diganti, dengan kode baru.
- `cv_replace_all(old, new)` — sama seperti `cv_replace()` tapi ia akan menggantikan semua kemunculan dari konten lama, seperti fitur *find & replace all*, cocok untuk **konsistensi ketika perubahan nama**, istilah tertentu, dll. Walaupun jarang digunakan.

Sebenarnya ada banyak tools lagi yang digunakan di CV Tailor, seperti untuk kebutuhan *long term memories*, dan kebutuhan yang mendukung agar platform *user friendly* juga. **Ini akan terus berkembang seiring kebutuhan.**

Cuss cobain, **copy semua konten isi resume lamamu dan paste langsung ke chat-nya!** Let AI do the rest for ya :)
