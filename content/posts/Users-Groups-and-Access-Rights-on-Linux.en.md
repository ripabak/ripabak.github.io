---
title: "Users, Groups, and Access Rights on Linux"
date: 2023-06-21T00:00:00+07:00
tags: ["Linux"]
author: "Me"
showToc: true
TocOpen: false
draft: false
hidemeta: false
comments: false
description: "A short explainer on users, groups, and how permissions tie everything together on Linux"
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


On Linux, the whole "users and permissions" thing is one of the first concepts you have to internalize. A user is basically any account registered on the system, and permissions decide what that account can execute, read, or modify.

Here’s the quick breakdown:
- **Regular user** – the daily driver account. Limited privileges, only touches files or commands granted by the system or the user.
- **Superuser/root** – the boss of the OS. Root can touch anything, anywhere, any time. Use it wisely.

Then we have **groups**. Think of a group as a bundle of users that share the same permissions. Handy when you want multiple people (or processes) to access the same files or manage the same service.

Want to create a new group? Use `groupadd`:

```
$ groupadd webadmin
```

Need to plug a user into that group?

```
$ usermod -aG webadmin user1
```

Once the user is inside the group, they inherit whatever access the group has. So if a directory is locked down to `webadmin`, anyone in that group instantly gets the green light.



