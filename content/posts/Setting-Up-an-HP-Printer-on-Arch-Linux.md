---
title: "Setting Printer HP di Arch Linux"
date: 2023-06-21T00:00:00+07:00
tags: ["Linux"]
author: "Me"
showToc: true
TocOpen: false
draft: false
hidemeta: false
comments: false
description: "Checklist cepatku buat bikin printer HP jalan lancar di Arch Linux"
searchHidden: false
ShowReadingTime: true
ShowBreadCrumbs: true
ShowPostNavLinks: true
ShowWordCount: true
ShowRssButtonInSectionTermList: true
UseHugoToc: true
editPost:
    URL: "https://github.com/ripabak/ripabak.github.io/content/posts/Setting-Up-an-HP-Printer-on-Arch-Linux.md"
    Text: "Suggest Changes"
    appendFilePath: true
---


Ini alur setup cepet yang aku pake tiap kali butuh printer HP jalan di Arch:
1. Install CUPS: `sudo pacman -S cups`
2. Langsung enable service CUPS biar tetep jalan abis reboot: `sudo systemctl enable --now cups`
3. Install tools-nya HP: `sudo pacman -S hplip`
4. Colok printer via USB dan jalanin `sudo hp-setup -i`. Pastiin jalanin sebagai root, kalau enggak bakal dapet error klasik “No device selected/specified”. Kebanyakan prompt bisa dibiarin default—tekan Enter aja terus.
5. Install GUI helper: `sudo pacman -S system-config-printer`
6. Buka **system-config-printer**, klik **Add**, pilih printer kamu, dan pilih **HPLIP** sebagai backend.
7. Print test page buat mastiin semuanya ngobrol dengan bener.
8. Kalau aplikasi GTK (Evince, Okular, dll.) komplain soal printer ilang, install `gtk3` juga.
