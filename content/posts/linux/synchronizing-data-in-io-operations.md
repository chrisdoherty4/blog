---
title: "Synchronizing Data in Io Operations"
date: 2020-09-27T12:33:53-06:00

tags:
    - io
    - linux

categories:
    - Linux
    - TIL
---

When reading and writing data using a command such as `dd`, Linux can read data in larger chunks than requested buffering it in memory. The buffer is known as the _dirty cache_ and can be controlled using the `dirty_*` parameters defined in the Linux [sysctl vm documentation](https://www.kernel.org/doc/Documentation/sysctl/vm.txt).

Dependent on the `dirty_*` configuration, the amount of dirty cache data can grow quite large. For example, if you have a large amount of memory and the `dirty_background_ratio` and or `dirty_ratio` parameters are set your dirty cache will be quite large. A consequence of a large dirty cache is that commands that block until the buffer is flushed appear to be hanging for substantial periods of time as Linux has read lots of data into the dirty cache and is in the process of writing the data out.

## An hypothetical example of a hanging process

You want to `dd` a 5GB file onto a USB stick from an SSD using USB 2.0. You have 32GB of RAM and a 10% ratio setting resulting in a dirty cache size of 3.2GB. Finally, you specify a `bs=4M` limit on the `dd` command limiting read and write operations to 4MB at a time.

```shell
dd if=./5gb-file.iso of=/dev/sda bs=4M
```

As noted above, Linux may read data in larger chunks than specified and store the data in the dirty cache. Linux may read the 5GB file in larger chunks than 4MB at a time resulting in the dirty cache becoming full extremely quickly. However, USB2.0 has a write limit of 9.5MB/s resulting in a bottleneck. After the `dd` command finishs reading all data filling up the dirty cache it appears to hang.

## Inspecting the dirty cache

If we aren't performing any other extensive write operations we can inspect the dirty cache for the amount of data left to write.

```shell
cat /proc/meminfo  | grep Dirty | cut -d: -f2 | sed  "s/^[[:space:]]*//"
```

Retrieve your dirty cache settings with the following.

```shell
sysctl -a -r "^vm\.dirty.*"
```
