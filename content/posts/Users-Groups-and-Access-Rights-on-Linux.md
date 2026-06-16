---
title: "User, Group, dan Hak Akses di Linux"
date: 2023-06-21T00:00:00+07:00
tags: ["Linux"]
author: "Me"
showToc: true
TocOpen: false
draft: false
hidemeta: false
comments: false
description: "Penjelasan singkat soal user, group, dan gimana permission ngiket semuanya di Linux"
searchHidden: false
ShowReadingTime: true
ShowBreadCrumbs: true
ShowPostNavLinks: true
ShowWordCount: true
ShowRssButtonInSectionTermList: true
UseHugoToc: true
editPost:
    URL: "https://github.com/ripabak/ripabak.github.io/content/posts/Users-Groups-and-Access-Rights-on-Linux.md"
    Text: "Suggest Changes"
    appendFilePath: true
---


Di Linux, urusan "user dan permission" itu salah satu konsep pertama yang harus kamu pahami. User itu pada dasarnya akun apa aja yang terdaftar di sistem, dan permission nentuin apa yang bisa dilakuin, dibaca, atau diubah sama akun itu.

Ini ringkasannya:
- **Regular user** – akun harian. Privileges terbatas, cuma bisa nyentuh file atau perintah yang diizinkan sistem atau user itu sendiri.
- **Superuser/root** – bosnya OS. Root bisa nyentuh apa aja, di mana aja, kapan aja. Pakai dengan bijak.

Terus kita punya **groups**. Bayangin group kayak sekelompok user yang berbagi permission yang sama. Berguna banget pas kamu mau beberapa orang (atau proses) ngakses file yang sama atau ngelola service yang sama.

Mau bikin group baru? Pakai `groupadd`:

```
$ groupadd webadmin
```

Butuh masukin user ke group itu?

```
$ usermod -aG webadmin user1
```

Begitu user ada di dalam group, mereka mewarisi akses apa aja yang dipunya group. Jadi kalau ada direktori dikunci buat `webadmin`, siapa aja di group itu langsung dapet lampu hijau.
