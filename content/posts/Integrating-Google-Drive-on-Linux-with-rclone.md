---
title: "Integrasi Google Drive di Linux dengan rclone"
date: 2023-07-08T00:00:00+07:00
tags: ["Linux"]
author: "Me"
showToc: true
TocOpen: false
draft: false
hidemeta: false
comments: false
description: "Panduan cepat gimana caraku nyambungin Google Drive ke Linux pakai rclone biar rasanya native"
searchHidden: false
ShowReadingTime: true
ShowBreadCrumbs: true
ShowPostNavLinks: true
ShowWordCount: true
ShowRssButtonInSectionTermList: true
UseHugoToc: true
editPost:
    URL: "https://github.com/ripabak/ripabak.github.io/content/posts/Integrating-Google-Drive-on-Linux-with-rclone.md"
    Text: "Suggest Changes"
    appendFilePath: true
---


Langsung aja ya. Sebelum mulai yang aneh-aneh, pastiin rclone udah keinstall. Pakai package manager apa aja yang ada di distromu—kalau di Arch punyaku cuma `sudo pacman -S rclone`.

### Buat remote Google Drive
1. Jalankan `rclone config` di terminal.
2. Pilih `n) New remote` → ketik `n` dan tekan Enter.
3. Kasih nama. Aku namain `gdrive-ripa`—pilih apa aja yang masuk akal buat kamu.
4. Pilih **Google Drive** sebagai tipe storage (di menuku itu opsi 18).
5. Kosongin `client_id` dan `client_secret` kecuali kamu punya OAuth credentials sendiri.
6. Buat scope, aku pilih opsi 1: **Full access to all files (drive)**.
7. Skip `service_account_file` kecuali kamu tau emang butuh itu.
8. Pas ditanya soal advanced config, aku jawab `n) No`.
9. Jawab `y` buat buka browser window buat autentikasi, terus sign in pakai akun Google yang mau disambungin ke rclone.
10. Aku ga pake Shared Drives, jadi aku jawab `n) No` juga ke prompt itu.
11. Konfirmasi semuanya dengan `y) Yes this is OK` pas rclone ngerangkum remote-nya.

Kamu harusnya lihat kayak gini sekarang:

```
Current remotes:

Name                 Type
====                 ====
gdrive-ripa          drive

e) Edit existing remote
n) New remote
d) Delete remote
r) Rename remote
c) Copy remote
s) Set configuration password
q) Quit config
e/n/d/r/c/s/q> q
```

Tekan `q` (atau `Ctrl+C`) buat keluar dari config tool.

### Mount remote-nya
1. Buat folder yang bakal jadi view “Google Drive” kamu. Aku pake `mkdir ~/Google-Drive`.
2. Mount remote ke folder itu:

   ```
   ripa@arch:[~]$ rclone mount gdrive-ripa: ~/Google-Drive/
   ```

Biarkan terminal window itu tetep kebuka—`rclone mount` jalan di foreground. Cek isi `~/Google-Drive/` dan kamu harusnya lihat isi yang sama kayak di Drive aslimu. Kalau semuanya kelihatan oke, selamat, mount-nya berhasil.

- Buat unmount, tinggal tutup terminal atau tekan `Ctrl+C` di window yang lagi jalanin `rclone mount`.
- Pengen jalan di background? Tambahin `&` ke perintahnya:

  ```
  ripa@arch:[~]$ rclone mount gdrive-ripa: ~/Google-Drive/ &
  ```

Udah gitu aja—sekarang Google Drive kamu berlaku kayak folder lain di sistemmu.
