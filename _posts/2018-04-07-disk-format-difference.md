---
title: 磁盘格式：FAT32、NTFS、exFAT 和 APFS
date: 2018-04-07 23:44:06
tags: 
  - 计算机基础
---

最近，购买了一个移动外盘，在 macOS 使用作为数据备份和 Time Machine 备份的。在使用的过程中，经过 macOS 的格式，不能在 Windows 上面正常使用，所以，想写一篇文章来详细介绍下几个主流的磁盘格式。

## 一、FAT 32

FAT 32（File Allocation Table）可以将一个大硬盘定义成一个分区而不必分为几个分区使用，大大方便了对磁盘的管理。但是随着分区的扩大，运行速度要逐渐变慢。

在 FAT 32 文件系统中，最大的单个文件限制为 4GB。

## 二、NTFS

NTFS（New Technology File System）是由 Microsoft 公司开发的专用文件系统，Windows NT 家族的操作系统所在的盘符必须格式化为 NTFS 文件系统，在 4096 簇环境下。它取代了老式的 FAT 文件系统。

在 NTFS 中，提供了`长文件名`、`数据保护`、和`恢复`。最长的文件名可支持长达 256 个字符，单个分区大小可达到 2TB。NTFS 通过使用标准的事务处理日志和恢复技术来保证了分区的一致性。

总的来说，NTFS 格式有如下的优点：

* 提供了文件加密，具有更安全的文件保障。
* 有着良好的磁盘压缩功能。
* 支持最大达 2TB 的大硬盘。
* 更加完善的权限管理。
* 使用 B-Tree 结构，在访问大型文件时，速度更快。
* 可以直接读写压缩文件，而不需要压缩软件。
* 支持磁盘配额。

## 三、exFAT

exFAT（Extended Allocation Table File System），也称作 FAT 64，是比较新的文件系统格式。为了解决 FAT 32 等不支持 4GB 及其更大的文件而推出。

exFAT 格式在 FAT 32 和 NTFS 格式之间的一种折中方案。

## 四、APFS

APFS（Apple File System），是 Apple 公司发布的新的文件格式，用于替代之前使用的 HFS+ 格式。

APFS 的主要功能为`加密`，并且更加优化了磁盘的存储算法，大大降低了文件的大小。

## 五、Q & A

#### Q：FAT 32 格式下，一个目录可容纳多少文件？

A：在 FAT 32 格式下，一个目录能够容纳 65536 个文件。

#### Q：FAT 32 的兼容性？

A：在 DOS 系统下，可以直接访问 FAT 32 分区，在 macOS 下，也可以直接访问。

#### Q：FAT 32 具有碎片化嘛？

A：FAT 32 在文件删除后写入新资料，FAT 不会将档案整理成完整片段再写入，长期使用会使档案变的逐渐分散，而减慢了读写速度，当然，也会产生碎片化。

#### Q：NTFS 的兼容性？

A：NTFS 在 DOS 系统下不能直接访问，需要借助第三方工具才能进行正常的读和写，但是 NTFS 在 macOS 系统下能进行正常的读操作，写操作也是需要借助第三方工具才能完成。

#### Q：NTFS 单个文件的限制是多少？

A：NTFS 原则上单个文件是没有限制的，可以达到分区的大小。

#### Q：使用 U 盘的时候，采用哪种格式比较好？

A：推荐采用比较新的 exFAT 格式，可以在 Windows、Linux、macOS 三平台上进行读写操作，并且支持单个文件 4GB 以上。

