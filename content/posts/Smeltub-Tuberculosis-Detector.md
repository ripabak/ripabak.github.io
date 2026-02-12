---
title: "Smeltub Tuberculosis Detector"
date: 2025-01-21T11:30:03+00:00
# weight: 1
# aliases: ["/first"]
tags: ["Projects", "Smeltub", "Machine Learning", "AI", "Research", "Tuberculosis", "Detector"]
author: "Me"
# author: ["Me", "You"] # multiple authors
showToc: true
TocOpen: false
draft: false
hidemeta: false
comments: false
description: "Smeltub Tuberculosis Detector"
canonicalURL: "https://canonical.url/to/page"
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
    image: "images/smeltub/smeltub-components.png" # image path/url
    alt: "Smeltub Tuberculosis Detector" # alt text
    caption: "Smeltub Tuberculosis Detector" # display caption under cover
    relative: false # when using page bundles set this to true
    hidden: false # only hide on current single page
editPost:
    URL: "https://github.com/riparuk/riparuk.github.io/content/posts/Smeltub-Tuberculosis-Detector.md"
    Text: "Suggest Changes" # edit text
    appendFilePath: true # to append file path to Edit link
---

## Ideation
Deteksi tuberculosis dari napas? sound impossible right? itu ketika pertama kali aku mendengar nya.

Ceritanya bermula dari salah satu teman dekat aku ngajak aku join dalam sebuah lomba yang berskala nasional di Indonesia, Di waktu itu, kami sebenarnya juga belum punya ide apa yang mau kami buat, terdengar seperti jebakan ya wkwk, but its okay, kami diskusi.. hari demi hari kami cari tahu apa yang sekiranya berpotensi untuk menang dan bisa kami buat dan ide nya terus berubah-ubah.

Ketika kita ingin membuat suatu ide itu bisa terjadi, it should be make sense right (?), itulah kenapa .. kami baca banyak paper, cari tahu apa yang pernah orang lain lakukan, bagaimana metodologi nya, dan seterusnya. 

Sampai akhirnya, kami membuat keputusan ingin fokus ke bidang kesehatan dan kami memilih untuk membuat tuberculosis detector dari napas. *Sounds impossible at the time if you just heard about it, but this decision its actually possible, I will tell you why when I'm done with this post.*

Setelah baca dari beberapa sumber, ide nya adalah kami perlu napas dari seseorang dan menganalisa VOCs di dalam napas nya, ada beberapa alat yang serupa pernah dibuat sebelumnya terkait deteksi penyakit dari napas itu disebut *e-nose*, bisa untuk deteksi PPOK, diabetes, dan masih banyak lagi, dari sini kami paham kalau itu juga mungkin jika di terapkan ke penyakit tuberculosis. Di beberapa jurnal juga kami menemukan bahwa orang yang terjangkit tuberculosis punya VOCs dalam napas nya yang berbeda dengan orang yang sehat, dari situ kami kesimpulkan bahwa its possible [[1](https://doi.org/10.1016/j.tube.2006.03.004), [2](https://doi.org/10.1038/s41378-023-00594-0)].

Sederhananya seperti itu, sekarang muncul permasalahan. Kita udah tahu apa saja VOCs yang bisa digunakan untuk mengidentifikasi tuberculosis, tapi bagaimana kita bisa menganalisa VOCs tersebut? Mungkin kamu berpikir kan bisa pakai sensor (?), tapi tahu ga, alat untuk deteksi spesifik VOCs sesuai dengan penelitian yang kami dapat itu mahal banget.... sedangkan dana yang kami punya sangat tidak mungkin untuk bisa membeli alat tersebut, alat itu namanya GC/MS (Gas Chromatography–Mass Spectrometry). 

Di sini lah the power of machine learning di butuhkan. Ide kedua nya adalah ... kasarnya kami tidak perlu tahu VOCs spesifik nya, tapi kami bisa memanfaatkan algoritma machine learning untuk bisa membaca pola dari VOCs tersebut. Pola-pola itu dari data napas yang di berikan ke algoritma machine learning untuk membuat model yang bisa mengidentifikasi tuberculosis dari napas. 

Konsep nya seperti hidung kita, ketika kita mencium bau durian misalnya, dari sensor hidung yang kita miliki dan kemudian di kirim ke otak data nya, kita tahu itu bau durian, tapi kita tidak tahu menahu terkait VOCs nya apa saja. Karena itu, kami tidak perlu menggunakan alat GC/MS tapi cukup sensor-sensor pendeteksi gas yang murah dari jenis Metal Oxide Sensor yaitu MQ-2, MQ-3, MQ-4, MQ-6, MQ-7.

## Prototyping
Pengerjaan dimulai dari desain dan membuat alat nya, ini proses yang cukup lama dan penuh trial dan error, aku rasa tidak mungkin jika di ceritakan secara detail, alatnya bisa di lihat seperti pada gambar ini.

![Components](images/smeltub/smeltub-components.png)

Prototype nya sudah jadi, sekarang adalah waktunya kita mencoba alat tersebut. Langkah selanjutnya disini adalah kita perlu mengumpulkan data nya dari pasien yang terjangkit tuberculosis dan pasien yang sehat. 

Jujur ini adalah proses yang cukup panjang dan lebih melelahkan daripada membuat alatnya. Kami pergi dari satu rumah sakit ke rumah sakit, puskesmas, bahkan sampai datang langsung ke rumah untuk mendapatkan sampel napas nya, pastinya dengan protokol kesehatan yang sesuai prosedur ya. Kesalahan pada prototipe terkadang tidak dapat dihindari, terkadang habis baterai, ada masalah di server, atau bahkan ada masalah di prototipe itu sendiri. Tapi disini aku sangat belajar banyak hal, sangat worth it.

Aku di tim ini bertanggung jawab bagian Software sistem dan AI model nya, yang aku lakukan disini adalah menyediakan tempat pengumpulan data nya, jadi aku membuat sebuah sistem yang dimana sistem tersebut dapat menerima data napas dari prototype melalui RestFul API, dan di sistem tersebut juga proses pe-label-an di lakukan yang dapat di akses melalui website.

![Website](images/smeltub/smeltub-website.png)

## Machine Learning Model
Data sudah dikumpulkan dan dirasa cukup, sekarang adalah waktunya membuat machine learning model nya.

Ini tahap tidak kalah capek juga dari tahap sebelumnya, di sini aku kerja sendiri, bukan secara tim, aku harus membaca berbagai artikel untuk cari tahu cara yang optimal agar akurasi model nya bagus dari data yang sudah di kumpulkan sebelumnya.

Jadi gini, masing-masing sampel napas itu di kumpulkan selama 2 menit, nah disitu masing-masing sensor akan punya nilai nya tersendiri, data tersebut akan melewati tahap preprocessing sebelum di masukkan ke model, di antaranya menghapus outliers dan filtering.

Setelah data dibersihkan, tahap selanjutnya adalah pemilihan fitur, berdasarkan hasil baca-baca artikel juga, di sini fitur yang di ekstrak dari data tersebut berupa koefisien korelasi Pearson dan statistik deskriptif sederhana.
- Max
![Max](images/smeltub/smeltub-max-formula.png)
- Min
![Min](images/smeltub/smeltub-min-formula.png)
- Range
![Range](images/smeltub/smeltub-range-formula.png)
- Pearson Correlation Coefficient
![Pearson Correlation Coefficient](images/smeltub/smeltub-correlation-formula.png)

Saat menggunakan fitur-fitur tersebut untuk merepresentasikan informasi dari
sampel data napas secara keseluruhan (120 detik), akurasi model yang dihasilkan tidak optimal, model tidak dapat membedakan dengan baik antara pasien TB dan individu sehat.

Aku coba baca lagi ke beberapa literatur [[3](https://doi.org/10.22266/ijies2024.0430.56), [4](https://doi.org/10.1109/ACCESS.2023.3291451)], dan menemukan ada yang menggunakan teknik sampling, simple nya kita tidak ambil semua data nya, tapi ambil data yang paling optimal, dan hasil nya akurasi meningkat, dari 2 menit data tersebut, hanya 30/40 detik di awal yang digunakan / terbaik nya. 

Artinya, kalau alat ini siap digunakan nantinya, sample napas yang di kumpulkan tidak harus nunggu lagi selama 2 menit, tapi cukup 1/4 nya saja.

Fitur-fitur ini kan punya dimensi data yang besar, bayangin aja satu data napas di dalam nya ada data sinyal dari 5 gas sensor, sebelum di berikan ke model, data tersebut akan di proses dulu dengan metode Principal Component Analysis (PCA), metode ini akan mengurangi dimensi data tersebut menjadi 2 dimensi dan mudah juga untuk di visualisasikan.

Hasil nya seperti ini
![PCA](images/smeltub/smeltub-model-visual.png)

Dan akurasi model yang dihasilkan adalah 
![Accuracy](images/smeltub/smeltub-accuracy.png)

Bisa di nilai sendiri lah ya algorithm yang cocok untuk kasus ini.

## Kesimpulan
> Sebelum ingin menutup tulisan ini, aku juga mau jujur dan menambahkan sesuatu, jika rekan-rekan ada baca tulisan ini dan ingin melakukan inovasi serupa atau  mirip, ada beberapa yang perlu di perhatikan. Hasil ini perlu pengujian lebih lanjut pastinya, yang di uji coba pada project ini terlalu fokus pada sisi akurasi model nya saja, banyak hal lain yang perlu di perhatikan, seperti konsistensi alat, sensor, prosedur pengumpulan data, jumlah data yang di kumpulkan, dan masih banyak lagi. Karena dana dan waktu yang terbatas sayang nya hal tersebut tidak kami lakukan dengan maksimal, tapi ini memberikan gambaran juga kepada kami bahwa project semacam ini sangat mungkin bisa dilakukan dan bermanfaat bagi orang banyak ke depannya.

## Achievement
Project seperti ini daripada nganggur kami juga per lombakan lho, Alhamdulillah kami dapat beberapa penghargaan.
![Achievement 1](images/smeltub/smeltub-achievement-1.jpeg)
![Achievement 2](images/smeltub/smeltub-achievement-2.jpeg)
![Achievement 3](images/smeltub/smeltub-achievement-3.png)

