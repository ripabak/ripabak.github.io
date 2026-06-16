---
title: "Photo Backup to AWS S3"
date: 2026-06-16T12:00:00+07:00
tags: ["AWS", "S3", "Backup", "Tips"]
author: "Me"
showToc: true
TocOpen: false
draft: false
hidemeta: false
comments: false
description: "Photo & video backup workflow from Sony ZV-1 to AWS S3 — organized by date, dirt cheap with Glacier lifecycle."
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

A while ago I wrote about [how to deal with a constantly full iPhone storage](/en/posts/solusi-storage-iphone-penuh/). Now I'm tackling a more fundamental problem: how to store photos as long-term memories.

To me, memories are priceless, you know. Through photos we can know our past, we can share with others, we can prove things — not only just a story, we can prove it.

I initially thought about using Google Drive for this. But since this is for the long term, I also need an affordable option. I also considered buying an SSD or HDD, started researching suitable options. But after digging into it, turns out buying just 1 HDD/SSD isn't best practice — you'd want RAID, which means buying more than one drive, which means even more cost.

Since I'm kind of obsessed with minimalism and… well, pretty lazy about dealing with physical storage, I eventually landed on **AWS S3**. The pricing is surprisingly cheap for personal use, especially with the lifecycle feature that automatically moves your data to cheaper storage over time.

## 💰 Cost (so you have an idea)

Here are the S3 prices in **ap-southeast-1 (Singapore)**:

| Storage Class | Price per GB/month |
|---|---|
| S3 Standard | $0.025 |
| S3 Glacier Flexible Retrieval | $0.0045 |
| S3 Glacier Deep Archive | $0.001 |

With a **64 GB** backup, I only pay about **$1.60/month** (around Rp 25,000) for S3 Standard. And it gets cheaper over time as files automatically move to Glacier, then Deep Archive — down to just **$0.064/month** (around Rp 1,000). It's basically free at that point haha.

Compare that to Google Drive at $2.79/month for 100 GB, flat forever. Old photos are something you rarely access — why pay premium for something you barely touch?

---

## 📸 Backup Workflow — Sony ZV-1 → AWS S3

### 🎯 Goal

Backup photos & videos from camera to the cloud with:
- Clean structure (organized by date)
- Low cost (using lifecycle to Glacier)
- Easy to reuse

---

### ☁️ 1. Set Up S3 Bucket

Create a bucket in AWS S3 as your backup target.

- **Region:** `ap-southeast-1` (Singapore)
- **Name:** unique, e.g. `photo-backup-rifa`

```bash
aws s3 mb s3://photo-backup-rifa --region ap-southeast-1
```

---

### 🔁 2. Enable Versioning

This prevents data loss if files get accidentally deleted or overwritten (whether from human error or a bad sync).

```bash
aws s3api put-bucket-versioning \
  --bucket photo-backup-rifa \
  --versioning-configuration Status=Enabled
```

---

### 💰 3. Set Up Lifecycle (to save money)

Lifecycle = automated rules to transition files to cheaper storage classes.

Recommended transitions:

```
30 days  → Standard-IA
90 days  → Glacier Flexible Retrieval
180 days → Glacier Deep Archive
```

Files start in Standard for fast access. Over time they automatically become cheaper archives.

---

### 🧰 4. Install Tools

We need two tools:

- **AWS CLI** — for uploading to S3
- **ExifTool** — for reading date metadata from photos & videos

Install on Mac:

```bash
brew install awscli
brew install exiftool
```

---

### 🔑 5. Configure AWS CLI

```bash
aws configure
```

Fill in:
- **Access Key ID** & **Secret Access Key** (create one in AWS IAM first)
- **Region:** `ap-southeast-1`
- **Output:** `json`

---

### 💾 6. Mount SD Card

Plug in the Sony ZV-1 SD card, check the mount location:

```bash
ls /Volumes
```

Example output: `/Volumes/SDCardCam`

---

### 📂 7. Camera File Structure

From the Sony ZV-1:

| Type | Path on SD Card |
|---|---|
| Photos | `DCIM/` |
| Videos | `PRIVATE/M4ROOT/CLIP` |

---

### 🗂️ 8. Organize Photos (by date)

ExifTool reads the `DateTimeOriginal` metadata from each photo and moves them into date-based folders.

```bash
exiftool '-Directory<${DateTimeOriginal}' \
  -d './foto-backup/%Y/%Y-%m-%d' \
  -r \
  /Volumes/SDCardCam/DCIM
```

Explanation:
- `-Directory<${DateTimeOriginal}` → move files based on the date the photo was taken (from EXIF)
- `-d './foto-backup/%Y/%Y-%m-%d'` → folder format: `year/year-month-day`, e.g. `2026/2026-06-16`
- `-r` → process all subdirectories (recursive)

---

### 🎥 9. Organize Videos

Videos usually don't have `DateTimeOriginal`, but they do have `CreateDate`. However, some video files don't have `CreateDate` either — when that happens, we fall back to `FileModifyDate` (the file's last modified date).

```bash
exiftool '-Directory<${CreateDate;$_=$FileModifyDate unless $_}' \
  -d './foto-backup/%Y/%Y-%m-%d' \
  -r \
  /Volumes/SDCardCam/PRIVATE
```

---

### ☁️ 10. Upload to S3

```bash
aws s3 sync ./foto-backup s3://photo-backup-rifa/zv1
```

Explanation:
- `./zv1` → local folder from the organize step, zv1 because its from camera
- `s3://photo-backup-rifa/zv1` → destination in your S3 bucket
- `sync` → only uploads new/changed files, no duplicates

---

### 🔁 11. Subsequent Backups

Just run the same command again:

```bash
aws s3 sync ./foto-backup s3://photo-backup-rifa/zv1
```

Only new files get uploaded. Fast and simple.

---

## 🧠 Workflow Summary

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

## 🔐 Best Practices

- **Enable versioning** — protects against accidental deletes
- **Keep a local backup too** (HDD/laptop) — don't rely solely on cloud
- **Test restore at least 1 file** — make sure your workflow actually works
- **Don't modify files directly on the SD card** — keeps EXIF metadata intact

---

## ✅ End Result

- Files neatly organized by date
- Automatic incremental backups
- Costs drop over time
- Safe from mistakes

---

Compared to paying premium prices for Google Drive for data you rarely access, this is such a relief. Now I can delete old captures from my phone without worry, because they're safely stored in S3 — and I can restore them anytime I need to.

Thank you for reading, hope this helps :)
