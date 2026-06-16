---
title: "Experiment LLM Salah Memahami Waktu"
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
description: "Eksperimen dengan LLM yang salah memahami waktu dan solusi yang diterapkan."
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

Beberapa waktu yang lalu, saya merasa workflow AI di app saya tidak sesuai dengan yang diharapkan. Ini terjadi beberapa kali walaupun dengan workflow yang sama yang pernah saya lakukan sebelumnya. Saya tidak mengubah apapun, asumsi saya model OpenAI dari `gpt-4.1-nano` kemampuannya berubah.

Yang terjadi di saya adalah output dari model LLM `gpt-4.1-nano` seringkali menghasilkan output yang tidak sesuai harapan. Dia memberikan output waktu yang salah, ini sangat jadi concern di aplikasi saya. Karena itu untuk sementara, saya move ke `gpt-5-nano` karena masih tergolong murah, *it worked*, ia lebih pintar, namun muncul masalah baru lagi.

`GPT-5-nano` itu butuh waktu yang lama dan butuh token besar (bisa 4x lebih besar) karena *reasoning*, padahal sistem saya tidak memerlukannya dan juga karena saya pakai sistem *reasoning* internal sendiri yang lebih sederhana dan token usage-nya kecil.

Saya coba mencari cara menonaktifkannya, ternyata sebagian model tidak support *reasoning*-nya dimatikan seperti `gpt-5-nano` itu sendiri. Saya consider `gpt-5-nano` berharap karena harganya juga lebih murah dibandingkan model nano sebelumnya (`gpt-4.1-nano`), namun masalahnya di sini adalah response-nya butuh waktu yang sangat lama karena *reasoning*.

Hanya beberapa model yang support dimatikan *reasoning* untuk versi GPT-5 ke atas, bisa cek di sini:
- [Need reasoning false option for GPT-5 update](https://community.openai.com/t/need-reasoning-false-option-for-gpt-5-update-gpt-5-1-solves-reasoning-issue/1351588)

Saya coba compare perbedaan harga dan spesifikasi di halaman detail models OpenAI, ini perbedaan model nano-nya, GPT-4.1 nano dan GPT-5 nano.

![gpt.4.1.nano](images/experiment-llm-salah-memahami-waktu/GPT.4.1.nano.png)
![gpt.5.nano](images/experiment-llm-salah-memahami-waktu/GPT.5.nano.png)

<!-- Image Placeholder: Perbedaan harga dan spesifikasi model nano -->

Ketika artikel ini ditulis, GPT-5 nano lebih murah dari harga INPUT-OUTPUT-nya namun *reasoning*-nya tidak bisa dimatikan. Kalau dipikir-pikir tidak jadi murah juga karena token-nya lebih besar dan juga butuh waktu yang lama untuk sebuah response.

Jadi saya ingin mencoba stay di `gpt-4.1-nano` agar harganya tetap murah dan cepat. Yang jadi permasalahan di sini adalah bagaimana cara agar saya bisa mengatasi masalah tersebut. Artikel ini akan membahas use case yang terjadi dan eksperimen yang saya lakukan.

## Kebutuhan dan Pendekatan

Kebutuhan saya adalah, saya butuh model tersebut memahami kapan waktunya dari text user input, seperti “tadi pagi”, “this morning”, “2 hari lalu”, dsb. Pendekatan yang saya lakukan selama ini adalah mengambil *current time* dari server dan memberikannya ke dalam konteks dan mengatur prompt template-nya agar mengikuti itu. Itu saya lakukan karena model LLM by default tidak mengetahui kapan waktu sekarang secara akurat karena model hanya tahu informasi dari data yang di-training.

Ini berfungsi sesuai ekspektasi dalam banyak kasus, namun baru-baru ini tidak demikian dan saya mencoba untuk cari tahu masalahnya dan saya diskusikan di sini.

Flow AI dari sistem saya kira-kira seperti ini:

![AI-flow](images/experiment-llm-salah-memahami-waktu/AI-flow.png)

Mungkin kamu tanya kenapa perlu generate user intent? Jawabannya karena jika output langsung ekstrak structured output to json, dalam banyak kasus hasilnya halusinasi. Makanya saya pakai pendekatan ini untuk memahami terlebih dahulu intent dari user secara terstruktur, kemudian baru saya ekstrak jadi JSON dengan informasi yang memang dibutuhkan. Bisa disebut *Chain-of-thought* sederhana untuk hal ini.

Prompt template untuk COT-nya saya meminta ke model agar seperti ini:

```text
…

follow this response format:
Wait, I need to focus & understand carefully when the expense should be added to add expenses. I think user wants to add (fill in the blank) from (indicate the time range), current time is (HH:mm DD/MM/YYYY). therefore the expense list is:
- 12,000 IDR for breakfast in the morning at 08:00 29/12/2023
- 16,000 IDR for lunch at an angkringan with chicken at 12:00 30/12/2023

…
```

## Masalah yang Ditemukan

Langsung saja, berikut beberapa contoh AI response yang tidak sesuai ekspektasi.

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

Bisa dilihat response dari AI-nya tidak dipancing untuk menyebutkan *current time* di user intent-nya, saya rasa di sini karena saya kurang strict instruksinya.

Eh ternyata ada log yang menyebutkan *current time* tetapi salah.

**User Input:** `Lunch for 60 twd`

**Generated User Intent:**
> Wait, I need to focus & understand carefully when the expense should be added to add expenses. I think user wants to add a lunch expense of 60 TWD from an unspecified time, current time is 12:52 31/12/2023. therefore the expense list is:
> - 60 TWD for lunch at the current time 12:52 31/12/2023

**AI Final:**
```json
{"expenses": [{"name": "Lunch", "amount": 60, "category": "Food", "currency": "TWD", "description": "", "transactionDate": "2023-12-31T04:52:00.000Z"}]}
```
**createdAt:** `2025-12-31 04:52:17.271`

“current time is 12:52 31/12/2023” whatt? how? Aku sebenarnya ada memasukkan current time date-nya sebagai konteks di system prompt-nya di awal, apakah server time-nya yang beda? hmm maybe.

Asumsi aku di sini ada 2:
1. Server *current time* yang salah.
2. LLM-nya halusinasi, karena contoh instruksinya saya menyebutkan 2023.

Aku coba lihat lagi data terbaru user intent-nya log-nya:

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

Wait.. what dia tidak generate dengan benar bagian current time-nya… hmm.
- LLM menulisnya dengan format yang salah
- LLM menulis ulang instruksinya

Tapi.. yang pada bagian ‘keychain’ bener lho current time-nya di user intent, tetapi … Final Output-nya juga masih salah ternyata, begini data log-nya:

**User Input:** `Keychain for 90 twd`

**Generated User Intent:**
> Wait, I need to focus & understand carefully when the expense should be added to add expenses. I think user wants to add a keychain purchased for 90 TWD, but no specific time mentioned. Therefore, the expense list is:
> - 90 TWD for keychain at current time 14:33 23/01/26

**AI Final:**
```json
{"expenses": [{"name": "keychain", "amount": 90, "category": "Shopping", "currency": "TWD", "description": "a keychain purchased", "transactionDate": "2023-01-26T06:33:00.000Z"}]}
```
**createdAt:** `2026-01-23 06:33:15.640`

Masalah baru lagi ya hmm…

Lihat, ternyata LLM-nya confused, “23/01/26” harusnya tanggal 23 tahun 2026, bukannya tanggal 26 tahun 2023, oh my god susah juga ngurus generated text kaya gini ya wkwk.

## Analisis Masalah

Asumsiku mengarah ke LLM-nya …
Apakah luxon bisa menghasilkan null sehingga LLM-nya tidak mendapatkan current time sekarang? Aku coba konsultasi ke ChatGPT tentang library ‘luxon’, katanya:

<!-- Image Placeholder: Penjelasan Luxon dari ChatGPT -->

Oke I take that as a fact ya.. kalau DateTime-nya ternyata invalid mungkin asumsi current time-nya menghasilkan 2023 karena server bisa jadi benar juga, makin bingung yaa…

Oke fine, asumsi aku sekarang dua-duanya punya potensi jadi masalah:
1. Current time yang salah, mungkin karena server-nya, aku pakai Vercel, mungkin ada hubungan karena serverless yang cold restart.
2. LLM-nya yang salah, ini mungkin karena akunya juga kurang strict dari sisi prompting-nya.

Aku tidak make sure apakah server time-nya yang beda, ini cukup ribet untuk dilakukan karena itu.

## Solusi yang Diterapkan

Solusi yang aku coba terapkan:
1. Paksa dari client mengirim waktu current time mereka, ini lebih minim untuk berubah aku rasa current time-nya, kecuali user mengubah time & date di device mereka.
2. Improve prompt template AI-nya, agar memaksakan ikut dengan instruksi bentuk response user intent-nya, saya menambahkan ini salah satunya.

![prompt-improvement](images/experiment-llm-salah-memahami-waktu/prompt-improvement.png)

Apakah masalah selesai? **TIDAKK**, I’m happy found another problem, lihat log-nya ini:

**User Input:** `Lunch for 60 twd`

**Generated User Intent:**
> Wait, I need to focus & understand carefully when the expense should be added to add expenses. I think user wants to add a lunch expense of 60 TWD, but I need to know the specific time when this happened. current time is 16:08 12/02/2026. therefore the expense list is:
> - 60 TWD for lunch at (time not specified) (12/02/2026)

**Final AI:**
```json
{"expenses": [{"name": "Lunch", "amount": 60, "category": "Food & Drink", "currency": "TWD", "description": "", "transactionDate": "2026-12-01T17:00:00.000Z"}]}
```

Lagi-lagi LLM salah memahami format tanggal, harusnya tanggal 12 bulan 2, LLM confused dan menganggap kalau transactionDate tanggal 2 bulan 12, jadi salah transactionDate-nya.. it’s okay.. jadi sekarang tantangannya cara buat LLM-nya paham akan format date yang konsisten. Saya mengubah di sisi system prompt-nya:

Prompt template system saya masih belum menyesuaikan dengan ISO Format, jadi saya ubah menjadi seperti ini. Agar bisa lebih universal, bisa saja UTC+0, UTC+7, dsb.

![improve-response](images/experiment-llm-salah-memahami-waktu/improve-response.png)

Ternyata ada indikasi lain juga kenapa 2023, yaitu dari prompt saya sendiri memberikan contoh 2023, mungkin bisa bias ke arah sananya sehingga LLM-nya halusinasi.

Sekarang ini bisa lebih konsisten, saya coba dengan user input = “Lunch for 60 twd”:

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

## Kesimpulan

At least sekarang, output-nya lebih konsisten untuk masalah transaction date-nya, walaupun bisa dilihat ada hasil generate user intent yang tidak muncul kalimat current time-nya :(. At least sekarang hasilnya tidak halusinasi seperti sebelum-sebelumnya. Ini akan terus di-improve dan kalau aku menemukan hal yang menarik, aku akan share di blog pribadiku [ripabak.github.io](https://ripabak.github.io).

Kesimpulannya adalah, ketika bermain dengan LLM untuk case predicted output yang ketat seperti ini, prompt system-nya juga harus ketat, apalagi kalau menggunakan model yang cheap dan cepat. Setiap problem berkaitan dengan halusinasi LLM harus di-strict dari segi prompting-nya. Semoga bermanfaat.
