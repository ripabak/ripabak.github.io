---
title: "QIAI (Quick Assistant)"
date: 2025-07-22T00:00:00+07:00
tags: ["Projects", "AI", "LLM", "Terminal", "GPT", "Gemini"]
author: "Me"
showToc: true
TocOpen: false
draft: false
hidemeta: false
comments: false
description: "Asisten Perintah Terminal yang Terintegrasi dengan GPT & Google Gemini"
canonicalURL: "https://medium.com/@rifafaruqi/terminal-commands-assistant-integrated-with-gpt-qiai-quick-assistant-fb249a686fb8"
showReadingTime: true
ShowBreadCrumbs: true
ShowPostNavLinks: true
ShowWordCount: true
ShowRssButtonInSectionTermList: true
UseHugoToc: true
cover:
    image: "images/qiai/banner.png" # image path/url
    alt: "QIAI Terminal Command" # alt text
    caption: "QIAI Terminal Command" # display caption under cover
editPost:
    URL: "https://github.com/ripabak/ripabak.github.io/content/posts/Qiai.md"
    Text: "Suggest Changes"
    appendFilePath: true
---


Pas lagi ngerjain projek, kadang kita suka lupa beberapa perintah di terminal, kayak cara install paket, pakai option tertentu, atau filter penggunaan perintah grep misalnya.

![QIAI Terminal Command](images/qiai/qiai-terminalcommand.webp)

Kadang itu bikin males, harus googling dulu, lihat dokumentasi script, atau tanya chatGPT kalau mau instan. Padahal yang dicari cuma perintah terminal pendek cuma gara-gara lupa.


Biar simpel, aku bikin script yang gampang banget dipakai, terintegrasi sama GPT 3.5 API, dan ngehasilin output on-point yang kita butuhin, aku namain QIAI (Quick Assistant).

Sejak versi terbaru, QIAI juga support **Google Gemini** sebagai alternatif AI provider. Ini ngebolehin user buat milih atau ganti antara OpenAI dan Gemini tergantung preferensi, ketersediaan, atau pertimbangan biaya.

Kalau pakai chatGPT biasa, kita perlu sesuaikan beberapa hal kayak OS yang dipakai, bentuk output yang dimau, kalau enggak chatGPT bakal kasih jawaban panjang banget dan makan waktu.

![QIAI Example](images/qiai/qiai-example.webp)


Pas pakai QIAI, jadi lebih efisien, hasil yang dikasih lebih tertarget, itu karena QIAI (untuk v1.1.1) punya fitur:
- Jawaban Spesifik OS: Ngasih jawaban khusus buat sistem operasi yang lagi dipakai (Linux, macOS, Windows). (Windows belum dites sih)
- Multiple AI Providers: Dukung OpenAI GPT dan Google Gemini buat generate perintah terminal.
- Hemat Token: Prompt yang dioptimasi buat ngurangin penggunaan token, jadi lebih efisien. (Rata-rata 150 token per request)

OpenAI GPT API lumayan murah, pakai API buat QIAI ga makan biaya gede, ini biaya gpt 3.5 yang dipakai QIAI.

![QIAI GPT Price](images/qiai/qiai-gpt-price.webp)

Per 1 request, QIAI rata-rata pakai 150 token, jadi dengan $1 aja kamu bisa panggil request 13 ribu kali. lumayan murah kan, ga ada ruginya buat dicoba.

### Cara Install QIAI
Kamu bisa install gampang pakai npm secara global.

```
npm install -g qiai
```


Set openai api key buat akses gpt, jalanin:
```
qiai --set-openai-api-key your-api-key
```

Alternatifnya, kamu bisa konfigurasi Google Gemini dengan set API key-nya:

```
qiai --set-gemini-api-key your-gemini-api-key
```

Kamu bisa ganti atau kelola provider nanti tanpa install ulang QIAI.

### Cara Pakai QIAI
Tanyakan Pertanyaan, dengan jalanin:
```
qiai -q "pertanyaan kamu"
```

![QIAI Demo](images/qiai/qiai-demo.gif)

Makasih udah baca, semoga membantu :)

Kalau mau kolaborasi, atau butuh fitur lebih buat QIAI, bisa buat issue di repo github.

Link Github: https://github.com/ripabak/qiai
