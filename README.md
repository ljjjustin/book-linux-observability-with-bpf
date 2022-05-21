# Linux eBPF读书实践笔记

## 引言
《Linux Observability with BPF》一书的写作时间是2018年，这几年eBPF发展迅速，书中的一些内容可能已经过时了。
在阅读和实践的过程中，书中的有些示例可能会遇到编译的问题。这里我也只是记录在我的环境中，可以编译和实践的一些示例。
希望对于那些和我一样的eBPF新手有一定的帮助。

## 环境

我的试验环境如下：
* 操作系统：Ubuntu 20.04.4 LTS
* 内核版本：5.4.0-110-generic
* CPU架构：aarch64

1. 安装编译所需的软件包安装：
```bash
$ sudo apt install build-essential git make libelf-dev clang strace tar bpfcc-tools linux-headers-$(uname -r)
```

2. 下载内核源文件

```bash
$ sudo apt install linux-source
$ cd /usr/src/linux-source-5.4.0
$ sudo tar xf linux-source-5.4.0.tar.bz2
```

3. 编译安装libbpf

```bash
$ cd /usr/src/linux-source-5.4.0/linux-source-5.4.0/tools/lib/bpf
$ sudo make
$ sudo make install prefix=/usr/local
$ sudo cp /usr/local/lib64/libbpf.* /lib/aarch64-linux-gnu/
```

## 参考资料
