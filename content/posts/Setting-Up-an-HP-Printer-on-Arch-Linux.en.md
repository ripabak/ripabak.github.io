---
title: "Setting Up an HP Printer on Arch Linux"
date: 2023-06-21T00:00:00+07:00
tags: ["Linux"]
author: "Me"
showToc: true
TocOpen: false
draft: false
hidemeta: false
comments: false
description: "My quick checklist for getting an HP printer working nicely on Arch Linux"
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


Here’s the quick setup flow I use whenever I need an HP printer working under Arch:
1. Install CUPS: `sudo pacman -S cups`
2. Enable the CUPS service right away so it stays running after reboots: `sudo systemctl enable --now cups`
3. Install HP’s tooling: `sudo pacman -S hplip`
4. Plug the printer in via USB and run `sudo hp-setup -i`. Make sure you run it as root, otherwise you’ll get the classic “No device selected/specified” error. Most of the prompts can stay on their defaults—just keep hitting Enter.
5. Install the GUI helper: `sudo pacman -S system-config-printer`
6. Launch **system-config-printer**, click **Add**, pick your printer, and choose **HPLIP** as the backend.
7. Print a test page to confirm everything is talking properly.
8. If GTK apps (Evince, Okular, etc.) complain about missing printers, install `gtk3` as well.
