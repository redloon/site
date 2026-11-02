+++
title = "I should have changed my WSL2 resource config sooner"
date = 2026-03-17T05:00:00.000Z
tags = [ "linux", "wsl2", "windows" ]
+++

For a while I've been experiencing extreme slowness doing regular web browsing on my laptop, and it's been frustrating the heck out of me. I use my desktop and work laptop on the same Wifi network, and they're blazing fast, so what gives? I finally passed the threshold of discomfort enough to try to debug what's happening.

## Debugging Windows Slowness

To start, speed tests _(and subjective experience on other devices)_ made the network an unlikely culprit. Resource usage seemed like the next thing to explore, since it might be able to pull down web pages super fast but then not have enough CPU & memory to make Firefox snappy.

Lo and behold, my habit of leaving VSCode (and the WSL2 VM) running alongside 30+ tabs for days on end seems to have been the issue. WSL (Windows Subsystem for Linux) was hogging ton of resources that could have been put to better use. To compare:

{{< bootstrap-table "table table-dark table-striped table-bordered" >}}

|                | CPU   | Mem    | Swap   | Storage   |
| -------------- | ----- | ------ | ------ | --------- |
| Laptop         | 8     | 16GB   | n/a    | 1TB       |
| WSL2 (old)     | 🤬8   | 🤬8GB  | 🤬2GB  | n/a       |
| ✨ WSL2 (new)  | 5     | 5      | 4      | n/a       |

{{< /bootstrap-table >}}

Now I feel silly. It's been taking up fully 1/2 of my RAM, and is allowed to hog all my processors if it wants to. Cherry on top is the swap is only 2GB, but I have a fully 900+ GB of extra space I could use for swap, we don't need to be acting resource-constrained.

## A breath of fresh air - changing the WSL2 config

As you can see in the table above, I droppped things down to more sane defaults, while still allowing it to be somewhat beefy. Even running a ton of Firefox tabs and numerous VSCode windows with WSL2, I still hadn't hit more than 3.5 GB of RAM usage, so I felt comfortable using 5GB as breathing room. Also dropping CPUs down so I always have a few reserved for the host system, and then bumping up the swap since I have so much space it's basically free.

I feel quite silly it took me this long to investigate and try things out, but my laptop feels SOOO much snappier now in the browser.

The new `$HOME/.wslconfig` file ([latest saved in my dotfiles](https://github.com/redloon/dotfiles/blob/master/windows/wslconfig)):

```ini
[wsl2]
memory=5GB
# allow around half of 8 logical cpus
processors=5
swap=4GB # we have lots of disk, don't be stingy

[experimental]
autoMemoryReclaim=gradual
networkingMode=mirrored  # ports open in WSL reachable as localhost on Windows, VPN works; watch for port conflicts
dnsTunneling=true        # correct DNS resolution with mirrored networking
sparseVhd=true           # auto-compact virtual disk, minimal downside
```
