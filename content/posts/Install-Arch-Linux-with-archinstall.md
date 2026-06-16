---
title: "Install Arch Linux dengan archinstall"
date: 2023-07-08T00:00:00+07:00
tags: ["Arch Linux", "archinstall"]
author: "Me"
showToc: true
TocOpen: false
draft: false
hidemeta: false
comments: false
description: "Panduan instalasi Arch Linux yang ga ribet pakai script archinstall, biar setup ga jadi beban"
searchHidden: false
ShowReadingTime: true
ShowBreadCrumbs: true
ShowPostNavLinks: true
ShowWordCount: true
ShowRssButtonInSectionTermList: true
UseHugoToc: true
editPost:
    URL: "https://github.com/ripabak/ripabak.github.io/content/posts/Install-Arch-Linux-with-archinstall.md"
    Text: "Suggest Changes"
    appendFilePath: true
---


Arch Linux itu salah satu distro paling populer, dan ada alasannya. [AUR](https://wiki.archlinux.org/title/Arch_User_Repository) itu sangat memudahkan hidup, [wiki](https://wiki.archlinux.org/)-nya lengkap banget kayak manual kehidupan, dan hampir semua tweak yang kamu butuhin pasti ada dokumentasinya.

Aku udah sering gonta-ganti distro—Mint, LXDE spins, XFCE, sebut aja—tapi aku selalu balik lagi ke **Arch Linux**. Satu-satunya hal yang dulu bikin males adalah instalasi manualnya. Memang powerful sih, tapi makan waktu banget kalau kita cuma pengen sistem yang jalan.

Terus aku nemu script [archinstall](https://github.com/archlinux/archinstall). Ini cara install Arch yang "tetep keren, tapi sibuk". Ga perlu malu, ga ada gatekeeping—cuma alur terpandu yang tetep ngebolehin kamu ngatur sistem sesuai keinginan.

Archinstall udah ada di dalam ISO resmi dan nyediain UI terstruktur buat milih disk, paket, dan setup lainnya. Ini ga ngilangin kontrol kamu; cuma motong proses ngetik yang berulang-ulang.

### Sebelum menjalankan archinstall
- **Pertama**, siapin disk yang bakal kamu wipe. <span style="color:red">*Panduan ini mengasumsikan install bersih, jadi backup semua data penting—archinstall bakal format disk target tanpa ampun.*</span>
- **Kedua**, ambil ISO Arch terbaru, flash ke USB (Rufus, Ventoy, terserah), boot ke dalamnya, dan connect internet. Wi-Fi via [iwctl](https://wiki.archlinux.org/title/iwd) atau colok LAN—bebas, asal bisa download paket pas install.

Begitu masuk ke live environment dan connect, jalankan installer terpandu dengan ngetik **`archinstall`**.

```
    Set/Modify the below options

    > Archinstall language               set: English (100%)
    Keyboard layout                    set: us
    Mirror region                      set: []
    Locale language                    set: en_US
    Locale encoding                    set: utf-8
    Drive(s)
    Bootloader                         set: systemd-bootctl
    Swap                               set: True
    Hostname                           set: archlinux
    Root password                      set: None
    User account
    Profile                            set: None
    Audio                              set: None
    Kernels                            set: ['linux']
    Additional packages                set: []
    Network configuration              set: Not configured, unavailable unless setup manually
    Timezone                           set: UTC
    Automatic time sync (NTP)          set: True
    Optional repositories              set: []

    Save configuration
    Install (2 config(s) missing)
    Abort
    (Press "/" to search)
```

### Konfigurasi andalanku
- **Archinstall language**: `English (100%)`
- **Keyboard layout**: `us`
- **Mirror region**: biarin kosong atau pilih mirror deket kamu
- **Locale language**: `en_US`
- **Locale encoding**: `utf-8`
- **Drive(s)**: pilih disk target
- **Disk layout**: Wipe all (ya, ini bakal hapus semua di disk itu)
- **Bootloader**: `systemd-bootctl` atau `GRUB` (aku lebih suka `systemd-bootctl`)
- **Hostname**: nama apa aja yang bikin seneng—aku biasanya cuma `arch`
- **Root password**: aku skip dan ngandelin sudo
- **User account**: Pilih **“Add a user”**, ketik username (aku `ripa`), set password, jawab “yes” pas ditanya user jadi sudoer, terus **Confirm and exit**
- **Profile**: pilih sesuai kebutuhan—`desktop -> kde` kalau mau Plasma, dan sesuaikan driver grafis sama hardware kamu
- **Audio**: aku pake `pipewire`
- **Kernel**: `linux` bawaan udah cukup kecuali ada alesan khusus buat ganti
- **Additional packages**: tambahin ekstra kayak `chromium` kalau butuh langsung
- **Network configuration**: `Use NetworkManager`
- **Timezone**: `Asia/Jakarta`
- **Automatic time sync**: `True`
- **Optional repositories**: biasanya cuma `['multilib']` biar bisa install Steam nanti

Kalau semua udah kelihatan oke, pilih **Install**, ambil minum sambil nunggu nyalin file, terus reboot ke setup Arch barumu. Ga perlu ritual kuno lagi. 😄
