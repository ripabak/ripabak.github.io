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
description: "Aplikasi pencatatan pengeluaran pakai AI yang saya gunakan sehari-hari"
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
    URL: "https://github.com/ripabak/ripabak.github.io/content/posts/Traveed.md"
    Text: "Suggest Changes" # edit text
    appendFilePath: true # to append file path to Edit link
---

Ada banyak sekali aplikasi pencatatan keuangan di luar sana, yang menjadi concern saya sekarang ini adalah bagaimana membuat pencatatan pengeluaran itu tidak perlu se-manual mengisi banyak data pengeluaran seperti nama barang, harga, kategori, dan lain-lain.

Tapi sebelum itu, ada pengalaman yang menjadi alasan kenapa pencatatan pengeluaran itu perlu, yaitu ketika saya berada di luar negeri. Saya tinggal di Indonesia, biaya hidup di sini relatif lebih murah dibandingkan dengan di luar, karena perbedaan currency itu membuat saya sulit untuk membandingkan berapa pengeluaran saya so far.

Saat artikel ini ditulis, orang-orang sering pakai ChatGPT hampir untuk segala keperluan, ini udah jadi kebiasaan karena kemudahan dan efisiensi yang diberikan.

Saya berpikir, mengapa saya tidak menggunakan alur serupa juga untuk catat pengeluaran, jadi nantinya user hanya tinggal memberikan input berupa apa yang dia beli, harganya, dimana dia beli, dan kapan dia beli, dan seterusnya.

Dengan perkembangan LLM saat ini, aku percaya bahwa ini possible dan itu menjadi landasan kenapa saya membuat Traveed.

## Fitur-fitur

Beberapa fitur besar dari Traveed meliputi:

- **AI Expense Capture**: User hanya perlu memasukkan pengeluaran dengan mengetik dengan bahasa sehari-hari, misalnya "Beli mie seharga 15000 rupiah di warung bu ani", dan AI akan secara otomatis memisahkan input tersebut menjadi nama, deskripsi, jumlah, dan mata uang.

![Transactions](/images/traveed/transactions.png)
![Analytics](/images/traveed/analytics.png)

- **Prompt Tuning**: Ini berguna untuk menyesuaikan bagaimana AI memasukkan data pengeluaran, misalnya user ingin AI memasukkan data pengeluaran dengan format tertentu, bahasa tertentu, atau bahkan gaya tertentu maka user bisa mengatur prompt tuning untuk mengatur format tersebut.

![Prompt Tuning](/images/traveed/ai-personalized.png)

- **160+ Currencies Supported**: Mendukung berbagai mata uang termasuk IDR, USD, JPY, dan lainnya, dengan konversi otomatis kembali ke mata uang utama. Ini memudahkan user untuk membandingkan pengeluaran mereka di berbagai negara.

![Currencies](/images/traveed/currencies.png)

- **Custom Categories**: User bisa mengatur kategori pengeluaran nya, AI secara otomatis memahami kategori yang tersedia dan menggunakannya untuk mengkategorikan pengeluaran.

![Categories](/images/traveed/categories.png)

## Summary
<iframe src="https://traveed.app" width="100%" height="600" style="border:none; border-radius: 12px; box-shadow: 0 4px 20px rgba(0,0,0,0.1); margin: 20px 0;"></iframe>

Sebagai developer, saya menggunakan Traveed sebagai aplikasi pencatat pengeluaran saya sehari-hari, dan sejauh ini saya merasa puas dengan fitur-fitur yang ada dan terus melakukan improvement. Sekarang masih BETA dan dapat diakses gratis melalui [https://traveed.app](https://traveed.app).