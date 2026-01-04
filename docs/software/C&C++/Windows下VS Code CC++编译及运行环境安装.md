# Windows下VS Code C/C++编译及运行环境安装

# 1.安装VSCode和插件

## 1.1 安装VS Code

从VS Code官网下载vs code安装程序：[Download Visual Studio Code - Mac, Linux, Windows](https://code.visualstudio.com/Download)

根据自己的操作系统，选择需要下载的版本，这里下载Windows x64的版本：

![image-20250915192832241](./images/image-20250915192832241.png)

VS Code安装完成后，开始安装扩展

## 1.2 安装C/C++扩展

在Visual Studio Code中点击扩展按钮，在搜索框中输入c/c++，选择C/C++ Extension Pack安装：

1. 插件安装

1. C/C++ Extension Pack 	(C/C++扩展包，下载直接安装，它包含了 vscode 编写 C/C++ 工程需要的插件（C/C++、C/C++ Themes、CMake、CMake Tools和Better C++ Syntax等），和以前比不需要一个个找了。如果没有C/C++就单独安装
2. Chinese (Simplified) (简体中文) Language
3. Code Runner

![image-20250915193225622](./images/image-20250915193225622.png)

## 1.3 Code Runner插件调整：【扩展设置】，否则无法键盘输入

![image-20250915194010261](./images/image-20250915194010261.png)![image-20250915194047416](./images/image-20250915194047416.png)

![image.png](./images/1713172396795-085dcbba-e7d3-4c80-99ef-2f422e14854a.webp)

![image.png](./images/1713172428334-b662d124-d6f4-44cd-9ae8-fc413ece04cb.webp)

## 1.4 设置文字大小为18

【文件】-【首选项】-【设置】-【常用设置】

![image-20250915194407646](./images/image-20250915194407646.png)

## 1.5 VSCode提示快捷键调整:

1. 【文件】-【首选项】-【键盘快捷方式】-搜索（触发建议）

2. 将原来的Ctrl+Space修改为Alt+/

   ![image-20250915194620425](./images/image-20250915194620425.png)

3. 右键-【更改键绑定】

![image-20250915194717828](./images/image-20250915194717828.png)

##  1.6 设置自动保存代码

【文件】->勾选【自动保存】

![img](./images/1745029707502-ff3580fd-9300-4aa6-a7a7-71f9462d8011.png)

![img](./images/1745029730110-aaa37290-32c5-4c90-a4b3-49b720092726.png)

## 1.7 VScode中文乱码解决方案

因windows中文版系统cmd编码默认为GBK，而vscode默认新建文件的编码为UTF-8所以会出现中文乱码情况

​	更改vscode默认编码UTF-8为GBK，（*该法需确认系统编码环境为GBK格式*，cmd终端输入chcp可以查看当前系统默认编译器,65001代表UTF-8,936代表GBK;设置完仍需重启vscode否则仍会出问题）

修改默认新建文件和打开文件编码方式，【文件】-【首选项】-【设置】-【文本编辑器】-【文件】-【Encoding】 选择GBK

将此处的utf8改为gbk，即可使新建的文件均为gbk格式。

![image-20250915200359375](./images/image-20250915200359375.png)

已经写好的文件修改

![image-20250915195756212](./images/image-20250915195756212.png)

![image-20250915200610286](./images/image-20250915200610286.png)

![image-20250915200633381](./images/image-20250915200633381.png)

# 2.安装GCC

我们使用GCC C++来编译C，在Windows上通过安装mingw-w64来使用GCC

> ​	**GCC**是**GNU Compiler Collection**(GNU编译器套件)的简称，GCC早期的含义是**GNU C Compiler**(GNU C 编译器)，但是后来随着支持的编程语言增多，其含义扩展为现在的GNU编译器套件
>
> ​	**GNU**是**GNU's Not Unix!**(GNU不是Unix!)的缩写，这是一个递归缩写，即缩写包含了自己的名字。GNU的目标是创建一个完全自由(Free)的类Unix操作系统，GNU开发了很多项目，比如上面提到**GCC**、**GNU Bash**(一个命令行Shell)、GNU核心工具（比如**ls**:用于显示文件和文件夹列表， **cat**：用于显示文件内容， **cp**: 拷贝文件或文件夹）等。
>
> ​	**Linux**是由**Linus Torvalds**在1991年开发的一个操作系统内核（Linux = Linus + Unix），这是一个Free的操作系统内核，然后结合GNU提供的工具链，形成了现代广泛使用**GNU/Linux**操作系统（通常简称Linux）
>
> ​	**Unix** 是一个强大的、多用户、多任务操作系统，诞生于 **1969 年**（由 **AT&T 贝尔实验室** 的 **Ken Thompson** 和 **Dennis Ritchie** 等人开发）。它对现代计算影响深远，是 **Linux、macOS、BSD** 等系统的共同祖先。对的，Apple电脑的macOS是唯一仍在增长的“类Unix”桌面系统

### MSYS2

MSYS2 是一个基于 **Cygwin** 和 **MinGW-w64** 的 **Windows 开发环境**，提供 **类 Unix 命令行工具** 和 **原生 Windows 软件包管理**（通过 `pacman`），支持 **GCC、Clang、GDB** 等工具链，适用于 **C/C++ 开发、Shell 脚本、系统管理** 等场景。

从[MSYS2](https://www.msys2.org/)官网下载msys2安装程序：

![image-20250915190057595](./images/image-20250915190057595.png)

安装： Next

![image-20250915190149580](./images/image-20250915190149580.png)

选择安装路径：

![image-20250915190241465](./images/image-20250915190241465.png)

Next

![image-20250915190258697](./images/image-20250915190258697.png)

Next

![image-20250915190308167](./images/image-20250915190308167.png)

点击Finish按钮后立即运行MSYS2，当然之后也可以从开始菜单中运行MSYS2：

![image-20250915190423995](./images/image-20250915190423995.png)

![image-20250915190431674](./images/image-20250915190431674.png)

**安装gcc、gdb等工具**

Mingw-w64是Windows编译C/C++源代码的程序集，为了安装该软件，须执行如下命令，即可安装编译C/C++程序所需的编译工具如：gcc、g++、make等。此步骤安装的软件包较多，因此可能需要一定时间，取决于网络和电脑配置，约需3-5分钟。

```bash
pacman -S mingw-w64-x86_64-toolchain
```

![img](.\images\1760092939691-cf5555dc-6456-4387-8813-d4e3db37cb91.png)

补充：**(上面成功无需配置)**单独安装gcc和gdb

安装命令如下： 

```bash
pacman -S mingw-w64-ucrt-x86_64-gcc
```

![image-20250915191018782](./images/image-20250915191018782.png)

回答Y开始安装

![image-20250915191124240](./images/image-20250915191124240.png)

接着安装gdb

> **GDB**（**GNU Debugger**）是 GNU 项目开发的 **命令行调试工具**，主要用于调试 C、C++ 等编程语言的程序。

安装命令如下：

```bash
 pacman -S mingw-w64-ucrt-x86_64-gdb
```

![image-20250915191430547](./images/image-20250915191430547.png)

### 设置Path路径

> ​	Path 是 Windows 系统的核心环境变量。它通过指定一组特定的目录路径，极大地优化了命令查找过程。当用户输入命令时，系统无需在整个文件系统中搜索，而是仅在 Path 预设的这些目录中查找目标程序。这种机制显著提高了命令执行效率：若在所有 Path 路径中都未找到目标命令，系统会立即返回"命令未找到"的提示，避免了无谓的全盘搜索。
>
> 在Windows的开始按钮上点击右键，选择系统：
>
> ![image-20250915191604539](./images/image-20250915191604539.png)
>
> 在系统信息页面，点击**高级系统设置**：
>
> ![image-20250915192028000](./images/image-20250915192028000.png)
>
> 在弹出的对话框中点击**环境变量**按钮：
>
> ![image-20250915192049685](./images/image-20250915192049685.png)
>
> 有两种类型的环境变量，一个是当前用户的，一个是系统的，我们选择系统变量中的Path，然后点击编辑
>
> ![image-20250915192223130](./images/image-20250915192223130.png)
>
> 然后在接下来的对话框中点击新建，再点击浏览 在浏览文件夹的对话框中点击此电脑，C:\msys64\mingw64\bin（或前面安装MSYS2自定义的安装路径）,然后点击确定；
>
> ![image.png](.\images\1760093273672-61fd657f-5ab7-4eb2-8fc8-f47907a610c2.png)
>
> **(上面成功无需配置)单**独安装的gcc和gdb按照下面配置：
>
> 然后在接下来的对话框中点击新建，再点击浏览 在浏览文件夹的对话框中点击此电脑，C盘的msys64/ucrt64/bin（或前面安装MSYS2自定义的安装路径）,然后点击确定；
>
> 或者 
>
> 直接手动输入安装MSYS2自定义的安装路径 C:\msys64\ucrt64\bin：
>
> ![image-20250915192302374](./images/image-20250915192302374.png)
>
> 最终大概如下图：
>
> ![image-20250915192542767](./images/image-20250915192542767.png)
>
> 然后打开一个命令行工具（Windows键 + R，然后输入cmd回车），输入命令
>
> ```cmd
> gcc --version
> gdb --version
> ```
>
> ![image-20250915192619662](./images/image-20250915192619662.png)

到此位置，GCC安装和设置完成

# 3. C语言入门案例

> **1978 年**，C 语言之父 **Dennis Ritchie** 和 **Brian Kernighan** 在其经典著作《The C Programming Language》中首次使用 `printf("hello, world\n");` 作为第一个示例程序。
> 这本书成为编程教育的圣经，从此“Hello, World!”被广泛模仿，成为入门标配。

在磁盘上建一个Projects文件夹（嗯，这是大部分人的习惯），然后再在Projects下创建一个项目的文件夹，大概就是如下的样子：

![image-20250915195056998](./images/image-20250915195056998.png)

然后运行Visual Studio Code，在欢迎页面点击打开文件夹：

或者

从文件菜单中选择打开文件夹

或者执行命令 

cmd

code . 

![image-20250915193636954](./images/image-20250915193636954.png)

浏览到之前创建好的code/dat01文件夹：

在弹出的是否信任此文件夹中，选择信任：

![image-20250915193400311](./images/image-20250915193400311.png)

在左侧的资源管理器点击新建文件按钮：

或者在下面空白处点击鼠标右键，选择新建文件：

输入文件名：`helloworld.c`:

将下面代码拷贝到`helloworld.c`中，并保存：

![image-20250915193744043](./images/image-20250915193744043.png)

```c
#include <stdio.h>

int main(int argc, char const *argv[])
{
    printf("hello world 中文\n");
    return 0;
}

```

选中文件右键 Run Code

![image-20250915193843202](./images/image-20250915193843202.png)

输出hello world：

![image-20250915194204377](./images/image-20250915194204377.png)



