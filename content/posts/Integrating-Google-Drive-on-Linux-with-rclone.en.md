---
title: "Integrating Google Drive on Linux with rclone"
date: 2023-07-08T00:00:00+07:00
tags: ["Linux"]
author: "Me"
showToc: true
TocOpen: false
draft: false
hidemeta: false
comments: false
description: "A quick rundown on how I hook Google Drive into my Linux box with rclone so it feels native"
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


Let’s get straight to it. Before doing anything fancy, make sure rclone is installed. Use whatever package manager your distro ships with—on my Arch box it’s simply `sudo pacman -S rclone`.

### Create the Google Drive remote
1. Run `rclone config` in your terminal.
2. Pick `n) New remote` → type `n` and press Enter.
3. Give it a name. I called mine `gdrive-ripa`—choose whatever makes sense for you.
4. Select **Google Drive** as the storage type (in my menu it was option 18).
5. Leave `client_id` and `client_secret` empty unless you have your own OAuth credentials.
6. For the scope, I go with option 1: **Full access to all files (drive)**.
7. Skip `service_account_file` unless you know you need it.
8. When asked about advanced config, I answer `n) No`.
9. Say `y` to open a browser window for authentication, then sign in with the Google account you want to expose to rclone.
10. I don’t use Shared Drives, so I answer `n) No` to that prompt too.
11. Confirm everything with `y) Yes this is OK` when rclone summarizes the remote.

You should now see something like this:

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

Hit `q` (or `Ctrl+C`) to exit the config tool.

### Mount the remote
1. Create a folder that will act like your “Google Drive” view. I use `mkdir ~/Google-Drive`.
2. Mount the remote to that folder:

   ```
   ripa@arch:[~]$ rclone mount gdrive-ripa: ~/Google-Drive/
   ```

Keep that terminal window open—`rclone mount` runs in the foreground. Peek inside `~/Google-Drive/` and you should see the same contents as your actual Drive. If everything looks good, congrats, the mount works.

- To unmount, just close the terminal or hit `Ctrl+C` in the window that’s running `rclone mount`.
- Want it in the background? Add `&` to the command:

  ```
  ripa@arch:[~]$ rclone mount gdrive-ripa: ~/Google-Drive/ &
  ```

That’s it—now your Google Drive behaves like any other folder on your system.
   




