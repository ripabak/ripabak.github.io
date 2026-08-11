---
title: "ChatYourPage, AI Agent yang jawab pertanyaan dari konten website-mu"
date: 2026-08-11T12:00:00+07:00
tags: ["Projects", "AI", "LLM", "Agent", "RAG"]
author: "Me"
showToc: true
TocOpen: false
draft: false
hidemeta: false
comments: false
description: "AI Agent RAG yang dipasang cuma dengan satu baris kode — agent belajar dari konten website-mu, jawab pakai sumbernya, dan kamu bisa pantau siapa saja yang bertanya."
showReadingTime: true
ShowBreadCrumbs: true
ShowPostNavLinks: true
ShowWordCount: true
ShowRssButtonInSectionTermList: true
UseHugoToc: true
cover:
    image: "images/chatyourpage/banner.png"
    alt: "Landing page ChatYourPage"
    caption: "ChatYourPage — embed AI agent di website dengan satu baris kode"
    hiddenInSingle: true
editPost:
    URL: "https://github.com/ripabak/ripabak.github.io/content/posts/ChatYourPage.md"
    Text: "Suggest Changes"
    appendFilePath: true
---

Beberapa hari lalu aku sempat ngobrol sama temanku, dia pamerin web portofolio-nya. Sebenarnya cuma simple chatbot pada umumnya — yang akan menjawab pertanyaan berdasarkan pribadinya, *not really impressed* sih awalnya.

Yang bikin aku impressed adalah hal sederhana yang dia bilang ke aku: **lumayan ada yang cobain chatbot-nya, dia bisa lihat dari token-nya yang terpakai**. Dari sini muncul sebuah keinginan dan ide — kenapa aku gak buat aja ya? Aku juga pengen tahu berapa orang sih yang pakai chatbot-ku, dan sekalian belajar implementasinya.

Dari situ lahirlah [ChatYourPage](https://chatyourpage.com/).

Nih demo singkatnya:

{{< video src="videos/chatyourpage-demo-com.mp4" >}}

## Bukan cuma chatbot tanya-jawab

Beda dengan chatbot yang cuma bisa tanya jawab soal, lagi-lagi di project ini aku menjadikannya **AI Agent**. *Overkill?* maybe, tapi secara use case aku rasa cukup bisa menyesuaikan.

Chatbot-nya bukan lagi cuma tanya jawab, tapi **terkoneksi ke beberapa tools**, salah satunya `RAG` (*Retrieval Augmented Generation*) — jadi agent bisa menjawab pertanyaan dari database yang besar. User tinggal memasukkan informasinya: produk, service, jam operasional, rules, `FAQ`, atau apapun itu.

### Gimana RAG bekerja secara simpel

Secara sistem, dokumen tersebut akan dipecah menjadi beberapa *chunks* dan disimpan dalam *vector* dengan **model embedding**. Kemudian model embedding yang sama dipakai lagi untuk *searching* informasi tersebut — simple-nya seperti itu lah.

Nah, teknologi `RAG` ini yang sangat berguna, karena **agent bisa mencari sendiri informasi dari dokumen yang besar** — jadi penggunaan token juga lebih hemat, karena tidak semua informasi diberikan ke model secara langsung.

## Fleksibel soal knowledge base

[ChatYourPage](https://chatyourpage.com/) sudah kubuat agar support dokumen dari:

- `PDF` — upload file langsung
- `URL` — tinggal paste alamat halamannya
- *Plain text* panjang — copy-paste dokumen, notes, policies

Jadi cukup fleksibel untuk menambahkan *knowledge base*-nya nanti.

Btw, **website-ini sendiri juga udah pakai itu kok** — bisa dilihat ada button melayang yang bisa diklik (biasanya di kanan bawah). Tanya aja apa aja di sana.

## Agent-nya juga bisa eksplorasi halaman

Agent di ChatYourPage gak cuma `RAG`, dia juga **bisa tahu isi halaman website-nya sekarang** dan bisa *fetch* halaman tertentu untuk dapat konteks yang lebih dalam.

Biasanya cocoknya dipasang di *landing page*. Misal ada use case-nya halaman `FAQ`-nya terpisah, kamu bisa langsung tanya agent tentang `FAQ` dan ia akan eksplor — kalau di halaman utama tidak tersedia informasi tersebut. **Cukup fleksibel, bisa decide secara internal apa yang harus dilakukan.**

## Bisa dikonfigurasi biar menarik

Ada beberapa hal yang bisa dikonfigurasi agar agent chatbot-nya menarik dan menyesuaikan kebutuhan:

- **Personalisasi agent** — siapkan gambar template avatar
- **Personalisasi warna** chat panel widget-nya — tinggal pilih tema
- Di akhir, kita juga bisa **melihat penggunaan token**, jumlah requests, dan darimana request yang masuk ke agent tersebut — lumayan bisa tahu visitors kita kan ya :)

![Customize ChatYourPage agent](images/chatyourpage/customize-chatyourpage_opt.png)

## Cus cobain!

Bisa langsung dicoba — waktu artikel ini ditulis masih **BETA dan free**, jadi cus dicoba dan pasang langsung ke website-mu di [chatyourpage.com](https://chatyourpage.com/). Makasih udah baca, semoga membantu :)
