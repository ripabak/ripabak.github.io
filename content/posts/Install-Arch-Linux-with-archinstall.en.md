---
title: "Install Arch Linux with archinstall"
date: 2023-07-08T00:00:00+07:00
tags: ["Arch Linux", "archinstall"]
author: "Me"
showToc: true
TocOpen: false
draft: false
hidemeta: false
comments: false
description: "My not-so-manual walkthrough for installing Arch Linux with the archinstall script so the setup stops being a chore"
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


Arch Linux is easily one of the most popular distros out there, and for good reason. The [AUR](https://wiki.archlinux.org/title/Arch_User_Repository) is insanely convenient, the [wiki](https://wiki.archlinux.org/) reads like a full-blown manual for life, and pretty much every tweak you’ll ever need is documented somewhere.

I’ve hopped distro to distro—Mint, LXDE spins, XFCE, you name it—but I keep coming back to **Arch Linux**. The only thing that used to slow me down was the classic manual install. It’s powerful, sure, but wow it eats up time when all you want is a working system.

Then I met the [archinstall](https://github.com/archlinux/archinstall) script. It’s the “I’m still cool, but also busy” way to install Arch. No shame, no gatekeeping—just a guided flow that still lets you shape the system exactly how you like.

Archinstall ships with the official ISO and provides a structured UI for picking disks, packages, and the rest of the setup. It doesn’t remove control; it just cuts out repetitive typing.

### Before launching archinstall
- **First**, prep the disk you’re going to wipe. <span style="color:red">*This walkthrough assumes a fresh install, so back up anything important—archinstall will happily format the target disk.*</span>
- **Second**, grab the latest Arch ISO, flash it to a USB (Rufus, Ventoy, whatever works), boot into it, and get online. Wi-Fi via [iwctl](https://wiki.archlinux.org/title/iwd) or just plug in Ethernet—doesn’t matter, as long as you can pull packages during install.

Once you’re in the live environment and connected, fire up the guided installer by typing **`archinstall`**.

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

### My go-to configuration
- **Archinstall language**: `English (100%)`
- **Keyboard layout**: `us`
- **Mirror region**: leave empty or pick mirrors near you
- **Locale language**: `en_US`
- **Locale encoding**: `utf-8`
- **Drive(s)**: choose the target disk
- **Disk layout**: Wipe all (yes, this erases everything on that disk)
- **Bootloader**: `systemd-bootctl` or `GRUB` (I lean `systemd-bootctl`)
- **Hostname**: whatever name makes you happy—mine is often just `arch`
- **Root password**: I skip it and rely on sudo
- **User account**: Select **“Add a user”**, type a username (mine’s `ripa`), set a password, answer “yes” when it asks if the user should be a sudoer, then **Confirm and exit**
- **Profile**: pick what you need—`desktop -> kde` if you want Plasma, and match the graphics driver to your hardware
- **Audio**: I go with `pipewire`
- **Kernel**: stock `linux` is fine unless you have a reason to change
- **Additional packages**: toss in extras like `chromium` if you want them right away
- **Network configuration**: `Use NetworkManager`
- **Timezone**: `Asia/Jakarta`
- **Automatic time sync**: `True`
- **Optional repositories**: usually just `['multilib']` so I can install Steam later

When everything looks good, select **Install**, grab a drink while it copies files, then reboot into your fresh Arch setup. No ancient ritual required. 😄
