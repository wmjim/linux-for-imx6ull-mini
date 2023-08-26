# 开发板的第一个 APP 实验

## 获取代码

在 Ubuntu 终端上执行如下命令：

``` bash
~/items$ git clone https://e.coding.net/weidongshan/01_all_series_quickstart.git
```

该实验所用到的代码位于相对目录：

``` bash
01_all_series_quickstart/04_嵌入式Linux应用开发基础知识/source/01_hello
```

hello.c 的源码如下：

``` c
#include <stdio.h>


/* 执行命令: ./hello weidongshan
 * argc = 2
 * argv[0] = ./hello
 * argv[1] = weidongshan
 */

int main(int argc, char **argv)
{
     if (argc >= 2)
         printf("Hello, %s!\n", argv[1]);
     else
         printf("Hello, world!\n");
     return 0;
}
```

## 编译程序

在 Ubuntu 中可以执行以下命令编译、执行：

``` bash
$ gcc -o hello hello.c
$ ./hello
Hello, world!
```

上述命令编译得到的可执行程序 hello 可以在 Ubuntu 中运行，但是如果把它放到 ARM 板子上去，它是无法执行的。因为它是使用 gcc 编译的，是给 PC 机编译的，里面的机器指令是 x86 的。

``` bash
[root@100ask:/mnt/01_all_series_quickstart/04_嵌入式Linux应用开发基础知识/source/01_hello]# ./hello
-bash: ./hello: cannot execute binary file: Exec format error
```

给 ARM 板编译出 hello 程序，需要使用**交叉编译工具链**。

执行以下命令编译程序：

``` bash
$ arm-buildroot-linux-gnueabihf-gcc -o hello hello.c
```

这样编译出来的 hello 程序才可以在 ARM 板子上运行。

