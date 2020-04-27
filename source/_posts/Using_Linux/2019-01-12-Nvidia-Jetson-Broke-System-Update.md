---
date: 2019-01-12 00:00:00
title: "Nvidia JetPack Breaks Update on Host System"
category: ["Using Linux"]
---

Rencently I've been trying to update my Ubuntu16.04 using `apt` and it failed again and again. At first, I thought it was because of the GFW and the sources. It turned out that any effort on these settings was in vain.

At last, after analyzing the update logs and digging into my deepest memories, I found that it was because I installed Nvidia JetPack to use Jetson TX1/2. I had to remove a pile of things related to JetPack by following commands.

```bash
apt-get remove .*:arm64
dpkg --remove-architecture arm64
```
