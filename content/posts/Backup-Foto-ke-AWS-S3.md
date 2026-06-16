---
title: "Backup Foto ke AWS S3"
date: 2026-06-16T12:00:00+07:00
tags: ["AWS", "S3", "Backup", "Tips"]
author: "Me"
showToc: true
TocOpen: false
draft: false
hidemeta: false
comments: false
description: "Workflow backup foto & video dari kamera Sony ZV-1 ke AWS S3 — struktur rapi berdasarkan tanggal, biaya murah pakai lifecycle Glacier."
disableHLJS: true
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
    URL: "https://github.com/ripabak/ripabak.github.io/content/posts/Backup-Foto-ke-AWS-S3.md"
    Text: "Suggest Changes"
    appendFilePath: true
---

Beberapa waktu yang lalu aku cerita tentang [gimana cara ngatasin storage iPhone yang sering penuh](/posts/solusi-storage-iphone-penuh/). Nah sekarang, aku lagi dealing sama masalah yang lebih fundamental: gimana caranya nyimpen foto-foto sebagai memori untuk jangka panjang.

Buat aku, memori itu seberharga itu, *you know*. Dari foto-foto kita bisa tahu *our past*, kita bisa share ke orang lain, kita bisa buktiin sesuatu — *not only just a story, we can prove it*.

Awalnya aku mikir mau pakai Google Drive buat nyimpen ini. Tapi karena ini untuk jangka panjang, aku juga butuh opsi yang *affordable*. Aku sebelumnya juga mikir buat beli SSD atau HDD, mulai riset opsi-opsi yang cocok. Tapi setelah riset, ternyata itu bukan *best practice* kalau cuma beli 1 HDD/SSD — bagusnya pakai RAID, yang artinya butuh beli lebih dari satu dan biaya jadi makin banyak.

Nah, karena aku lumayan obsesi sama minimalis dan… ya gimana ya, lumayan males juga berurusan sama nyimpen storage fisik, akhirnya aku *landing* ke **AWS S3**. Harganya cukup murah buat kebutuhan personal, apalagi ada fitur *lifecycle* yang otomatis mindahin data ke storage yang makin murah seiring waktu.

## Biaya (biar ada bayangan)

Ini harga S3 di region **ap-southeast-1 (Singapore)** (asumsi 1 USD = Rp 17,600):

| Storage Class | Harga per GB/bulan |
|---|---|
| S3 Standard | $0.025 (Rp 440) |
| S3 Glacier Flexible Retrieval | $0.0045 (Rp 79) |
| S3 Glacier Deep Archive | $0.001 (Rp 18) |

Dengan backup **64 GB**, aku cukup membayar sekitar **$1.60 per bulan** (sekitar Rp 28,000-an) untuk S3 Standard. Dan semakin lama akan semakin murah karena otomatis dipindahkan ke Glacier, bahkan sampai ke Deep Archive — bisa menjadi cuma **$0.064 per bulan** (sekitar Rp 1,100-an). Udah kayak gratis kan rasanya wkwk.

Bandingin sama Google Drive yang $2.79/bulan (sekitar Rp 49,000-an) untuk 100 GB, dan itu flat terus gabisa makin murah. Padahal data foto lama itu kan jarang kita akses — *why pay premium for something you barely touch*?

---

## Backup Workflow — Sony ZV-1 → AWS S3

### Tujuan

Backup foto & video dari kamera ke cloud dengan:
- Struktur rapi (berdasarkan tanggal)
- Biaya murah (pakai *lifecycle* ke Glacier)
- Mudah digunakan ulang

---

### 1. Setup S3 Bucket

Buat bucket di AWS S3 sebagai tempat backup.

- **Region:** `ap-southeast-1` (Singapore)
- **Nama:** unik, misal `photo-backup-rifa`

```bash
aws s3 mb s3://photo-backup-rifa --region ap-southeast-1
```

---

### 2. Aktifkan Versioning

Tujuannya: mencegah kehilangan data kalau file terhapus atau ketimpa (entah karena human error atau sync yang salah).

Dari AWS Console:

1. Buka **AWS Console** → buka service **S3** → klik bucket `photo-backup-rifa` yang udah kamu buat
2. Pilih tab **Properties**
3. Scroll ke bagian **Bucket Versioning**, klik tombol **Edit**
4. Pilih **Enable**, lalu klik **Save changes**

Selesai. Sekarang setiap file yang di-upload atau di-overwrite bakal punya riwayat versi, jadi kamu bisa *restore* kapan aja.

---

### 3. Setup Lifecycle (biar hemat)

*Lifecycle* = aturan otomatis untuk pindahkan file ke storage yang lebih murah.

Rekomendasi transisinya:

```
30 hari  → Standard-IA
90 hari  → Glacier Flexible Retrieval
180 hari → Glacier Deep Archive
```

Awalnya file bisa diakses cepat di Standard. Lama-lama otomatis jadi arsip yang murah banget.

Cara setup di AWS Console:

1. Buka **AWS Console** → S3 → klik bucket `photo-backup-rifa`
2. Pilih tab **Management**
3. Di bagian **Lifecycle rules**, klik tombol **Create lifecycle rule**
4. Isi **Lifecycle rule name** (misalnya: `move-to-cheaper-storage`)
5. Di **Choose a rule scope**, pilih **Limit the scope to this prefix** → isi `foto-backup/` (atau kosongkan kalau mau berlaku ke semua isi bucket)
6. Di bagian **Lifecycle rule actions**, centang **Transition current versions of objects between storage classes**
7. Tambahkan transisi sesuai urutan:
   - Klik **Add transition** → pilih **Standard-IA** → **Days after object creation:** `30`
   - Klik **Add transition** → pilih **Glacier Flexible Retrieval** → **Days after object creation:** `90`
   - Klik **Add transition** → pilih **Glacier Deep Archive** → **Days after object creation:** `180`
8. Klik **Create rule**

---

### 4. Install Tools

Kita butuh dua tool:

- **AWS CLI** — buat upload ke S3
- **ExifTool** — buat baca metadata tanggal dari foto/video

Install di Mac:

```bash
brew install awscli
brew install exiftool
```

---

### 5. Setup AWS CLI

```bash
aws configure
```

Isi:
- **Access Key ID** & **Secret Access Key** (bikin dulu di IAM AWS)
- **Region:** `ap-southeast-1`
- **Output:** `json`

---

### 6. Mount SD Card

Colok SD card Sony ZV-1 ke Mac, cek lokasinya:

```bash
ls /Volumes
```

Contoh output: `/Volumes/SDCardCam`

---

### 7. Struktur File Kamera

Dari Sony ZV-1:

| Tipe | Path di SD Card |
|---|---|
| Foto | `DCIM/` |
| Video | `PRIVATE/M4ROOT/CLIP` |

---

### 8. Organize Foto (by tanggal)

ExifTool bisa baca metadata `DateTimeOriginal` dari tiap foto dan mindahin ke folder berdasarkan tanggal.

```bash
exiftool '-Directory<${DateTimeOriginal}' \
  -d './foto-backup/%Y/%Y-%m-%d' \
  -r \
  /Volumes/SDCardCam/DCIM
```

Penjelasan:
- `-Directory<${DateTimeOriginal}` → pindahkan file berdasarkan tanggal foto diambil (dari EXIF)
- `-d './foto-backup/%Y/%Y-%m-%d'` → format folder: `tahun/tahun-bulan-tanggal`, contoh `2026/2026-06-16`
- `-r` → proses semua folder di dalamnya (*recursive*)

---

### 9. Organize Video

Video biasanya nggak punya `DateTimeOriginal`, tapi punya `CreateDate`. Tapi kadang beberapa file video juga nggak punya `CreateDate` — kalau gagal, kita *fallback* ke `FileModifyDate` (tanggal file terakhir dimodifikasi).

```bash
exiftool '-Directory<${CreateDate;$_=$FileModifyDate unless $_}' \
  -d './foto-backup/%Y/%Y-%m-%d' \
  -r \
  /Volumes/SDCardCam/PRIVATE
```

---

### 10. Upload ke S3

```bash
aws s3 sync ./foto-backup s3://photo-backup-rifa/zv1
```

Penjelasan:
- `./foto-backup` → folder lokal hasil organize
- `s3://photo-backup-rifa/zv1` → lokasi di S3 bucket, zv1 karena dari kamera
- `sync` → hanya upload file baru / berubah, nggak duplikat

---

### 11. Backup Berikutnya

Tinggal jalankan lagi command yang sama:

```bash
aws s3 sync ./foto-backup s3://photo-backup-rifa/zv1
```

Hanya file baru yang akan di-upload. Cepet, simpel.

---

## Workflow Ringkas

```
SD Card
  ↓
ExifTool (organize by date)
  ↓
foto-backup/
  ↓
aws s3 sync
  ↓
S3 (auto lifecycle → Glacier → Deep Archive)
```

---

## Best Practice

- **Aktifkan versioning** — biar aman dari *accidental delete*
- **Simpan backup lokal juga** (HDD/laptop) — jangan cuma cloud
- **Test restore minimal 1 file** — *make sure* workflow lo bener
- **Jangan ubah file langsung di SD card** — biar metadata EXIF tetap utuh

---

## Hasil Akhir

- File tersusun rapi berdasarkan tanggal
- Backup otomatis *incremental*
- Biaya makin murah seiring waktu
- Aman dari kesalahan

---

Dibanding harus bayar mahal pakai Google Drive buat data yang jarang diakses, ini pilihan yang bikin lega. Sekarang aku bisa hapus *capture* lama di HP tanpa khawatir, karena udah aman tersimpan di S3 — dan nanti tinggal di-*restore* kalau sewaktu-waktu butuh.

Makasih udah baca, semoga membantu :)
