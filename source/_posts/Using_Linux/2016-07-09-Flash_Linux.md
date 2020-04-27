---
date: 2016-07-09 00:00:00
title: "Install Adobe FlashPlayer for Firefox on Ubuntu 14.04"
category: "Using Linux"
---

Though it is recommended using HTML5 in the field, Adobe FlashPlayer is still widely used. So it is of need to install it in our system. The main steps are as follows:

1. Download Adobe FlashPlayer from the [official website](https://get.adobe.com/cn/flashplayer/). I choosed .tar.gz version.
2. Extract the archive using `tar -zxvf install_flash_player_11_linux.x86_64.tar.gz`
3. Copy files to corresponding directories.

```bash
sudo cp libflashplayer.so /usr/lib/mozilla/plugins/
sudo cp -r ./usr/* /usr/
```