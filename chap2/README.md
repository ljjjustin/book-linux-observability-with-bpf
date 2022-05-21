# 编译

编写完BPF版的hello world程序后，通过clang编译，报如下的错误：

```bash
$ clang -O2 -target bpf -c hello.c -o bpf_hell.o
In file included from hello.c:1:
In file included from /usr/include/linux/bpf.h:11:
/usr/include/linux/types.h:5:10: fatal error: 'asm/types.h' file not found
#include <asm/types.h>
         ^~~~~~~~~~~~~
1 error generated.
make: *** [Makefile:3: all] Error 1
```
去`/usr/include`目录下找了一下，发现确实没有。google一番，发现在arm64位架构下，
头文件的路径是`/usr/include/aarch64-linux-gnu`，可以修改环境变量`C_INCLUDE_PATH`，
让clang去加载该环境变量所指目录下的头文件，这个问题就顺利结束。

光有helloworld程序还不够，bpf还需要loader把它加载到内核中。
在内核的源代码中，包含了libbpf，在配置环境的时候，我们已经编译安装了。此次，我们直接链接即可。

```bash
$ make
```

## 运行

编译没有报错的话，就会生成一个名为mon-exec的可执行文件。通过下面的命令执行：
```bash
$ sudo ./mon-exec
```
BPF程序只有root用户才能加载，普通用户所以加载时注意使用sudo，提升权限。
