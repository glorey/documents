# 面向Python和C++用户的最大熵工具包
  zhang le

## 第一章 它是什么
&emsp;&emsp;这是一个用C++语言编写的最大熵工具包，同时包含python接口。具体来说，它包含
* 条件最大熵模型
* L-BFGS参数估计
* GIS参数估计
* 高斯先验平滑
* C++ API
* Python扩展模块
* 文档和教程

&emsp;&emsp;如果你不了解什么是最大熵模型，可以参考第三章进行基本的了解，或者3.3小节来学习一些推荐的论文。

&emsp;&emsp;本手册参考最新版本工具包的功能。不同的发布版本之间的差异可以参考NEWS(大的更新)和ChangeLog(小的更新)。


### 1.1 使用许可
&emsp;&emsp;本程序库源自于早期尝试将[Java最大熵工具包](#http://maxent.sourceforge.net)转写为C++。因此它遵守与java最大熵工具包相同的许可。本程序库是一个免费软件，遵守LGPL许可(具体可以查看LICENSE文件，或者访问http://www.gnu.org/copyleft/lesser.html)。
本程序库将完整的源码和文啊的那个一起发布，同时欢迎大家进行贡献以及bug反馈。

### 1.2 待做事项
* orange绑定(beta版，参考python/orange/)
* IFS特征选择
* 磁场感应算法
* 非条件最大熵模型(随机场)
* 在本文档中引入更多关于最大熵模型的材料
* 重写ANSI C
* 常见Lisp绑定

### 1.3 已知问题
&emsp;&emsp;当你设置一个很小的高斯先验值(-g)时，LBFGS训练可能会停止。
此问题好像只在小训练数据量时出现。但是此问题可能的原因仍在调查。

# 第二章 编译和安装
## 2.1 系统要求
&emsp;&emsp;目前，此工具包可以在许多主流的操作系统中运行。包括POSIX环境，像GNU/Linux，FreeBSD，NetBSD，SunOS和Win32（Mac OS X目前不支持）。你需要一个合适的C++编译器来编译此工具包。以下的C++编译器已经经过测试。

* GNU C++编译器3.2版或以上（GCC 2.9X不支持），包括Win32上的Cygwin
* Win32上使用MinGW，GCC 3.2
* Borland免费c++编译器5.5版，包含STLPort 4.5.3
* 微软Visual C++ 7.1命令行工具
* 微软Visual C++ 7.1，包含STLPort
* Intel C++ 8.0，在Win32上，包含MSVC7.1的STL
* Intel Fortran编译器，Win32系统（为编译L-BFGS模块）

&emsp;&emsp;以下是为编译此工具包所需要的软件列表。

* Jam构建系统，用来构建整个工具包(已经包含)
* 一个合适C++编译器，包含STL，可以参考上面的列表
* Boost C++程序库（已经包含）
* Fortran编译器(最好是g77)用来编译L-BFGS模块（可选），如果没有Fortran编译器，LBFGS模块将被禁用。
* zlib程序库（可选）

&emsp;&emsp;[Jam](#http://www.perforce.com)是一个伟大的make替代品，
它会使得构建简单事情时很简单，构建复杂事情是可以控制。
它是一个紧凑的，便携式的以及功能更强的make。最近版本的Jam源码在```tools```文件夹中，在构建过程中自动构建。

&emsp;&emsp;[Boost](#http://www.boost.org)是一个高质量的C++模板库集合。特别地，可以检查一下boost的include文件夹是否在你的编译器查找路径中（通常在```/usr/local/include/boost```中）。

&emsp;&emsp;本程序包中包含一个完整boost库的子集，仅包含编译过程中使用到的头文件。它在大部分的平台上工作正常。
然而，如果你计划在其他的平台上构建此工具包，或者使用gcc之外的编译器，请下载完整版本的boost库，并将boost include文件夹放在编译器的cpp搜索路径中。

&emsp;&emsp;zlib用来创建压缩的二进制模型，压缩二进制模型比纯文本模型小很多，在运行过程中花费极少的加载时间。

### 2.2 构建C++库

&emsp;&emsp;在构建本程序库的核心部分之前，请检查一下前一小节中的软件列表，确保所有需要的软件已经安装并正确配置。

#### 2.2.1 在Unix平台上构建(Linux/\*BSD/SunOS等)
&emsp;&emsp;本软件已经在GNU/linux(内核2.4)，FreeBSD 4.8/4.9，NetBSD 1.62， SunOS 5.9系统上，在GCC 3.2/3.3/3.4下测试通过。
它也可能在其他的unix系统上工作正常，包括win32上的Cygwin。

&emsp;&emsp;首先解压tar包，将解压文件放在临时文件夹中

```sh
tar zxf maxent-version-number.tar.gz
```

&emsp;&emsp;进入maxent-version-number子文件夹，运行```configure```脚本来配置工具包。

```sh
$ ./configure
```

&emsp;&emsp;```configure```脚本会根据你的机器自动配置编译环境，包括正确的编译器，是否包含Fortran编译器，
以及是否安装Boost。

&emsp;&emsp;如果没有打印任何错误，你可以输入```make```开始构建。

```sh
$ make
```

&emsp;&emsp;然后会在优化模式下自动构建程序库。构建C++程序库会消耗一定的时间。
如果你的机器比较慢，你可以喝一杯茶。

&emsp;&emsp;你也可以选择构建并运行一系列测试样例，确保不会出现错误。

```sh
$ make unittest
```

&emsp;&emsp;然后进入```test/```文件夹，运行```runall.py```脚本。
如果测试通过，就可以安装本工具包（root用户）

```sh
# make install
```

&emsp;&emsp;默认情况下，此工具包将被安装在```/usr/local```文件夹下。
你可以通过以下指令覆盖默认的prefix到```/usr```

```sh
$ ./configure --prefix=/usr
```

&emsp;&emsp;如果你想debug本程序库，```--enable-debug```会构建一个
debug版本的程序库。```--disable-system-boost-lib```会强制使用程序库
中自带的boost库。当你的机器上安装的boost库不能工作时，此选项很有帮助。
运行```./configure --help```会列出```configure```脚本的选项列表。

### 2.2.2 在win32平台上构建(BCC/MSVC/Intel C++等)
&emsp;&emsp;本软件可以在win32平台上构建。支持多种C++编译器，需要包含Fortran编译器。如果你希望使用非GCC编译器构建此软件，你需要获取```jam.exe```（在本软件包的下载页面上提供）。

#### 2.2.2.1 Cygwin和MinGW
&emsp;&emsp;[Cygwin](#http://www.cygwin.com)提供一个在Win32 API上可以正常工作（但是很慢）的POSIX层。如果你已经安装了Cygwin，你可以参考unix构建小节中的具体指令。

&emsp;&emsp;[MinGW](#http://www.mingw.org)是一个win32版的GCC编译器。
除了核心的gcc-win32工具，MinGW提供了一个被称为MSYS的子系统，
该子系统包括一些基本的工具来运行一个shell环境。
你可以安装MinGW以及MSYS，这样可以无痛地在win32上编译此工具包。
详细的编译步骤和在unix上编译一样，只是这个过程会有一些慢。

#### 2.2.2.2 带有STLPort的Borland C++
&emsp;&emsp;本软件可以通过Borland发布的免费C++编译器来构建，当前版本是5.5，可以在http://www.borland.com/bcppbuilder/freecompiler 页面查找到。
然而，BCC5.5自带的默认STL不支持```hash_map```，你需要安装[STLPort](#http://www.stlport.com)。你需要注意的是最新版本的
STLport在BCC5.5上构建失败。你需要使用STLPort 4.5.3版本。

&emsp;&emsp;在Borland 5.5下的构建步骤：
* 确保你已经安装了STLport。（在bcc32.cfg和ilink32.cfg中设置STLPort的文件夹）。
* 设置环境变量```BCCROOT```到你的Borland C++安装目录。
* 在src文件夹输入```jam```指令来进行构建。

#### 2.2.2.3 微软Visual C++
&emsp;&emsp;微软Visual C++是Win32平台上应用最广泛的商业C++编译器。
为响应大家的需求，本工具包可以在MSVC7下构建（MSVC6也可能工作，但是没有测试）。
MSVC7在支持C++标准方面相比MSVC6有很大进步。因为MSVC7的默认STL中包含```hash_map```类，因此本软件是可以直接编译的。
但是，强烈建议使用[STLPort](#http://www.stlport.com)来替换MSVC7中的默认STL，因为前者更快。

&emsp;&emsp;MSVC下的构建步骤
* 设置环境变量MSVCDIR（不是MSVCDir）到你的Visual C++安装目录。
* 在src文件夹中输入```jam```来进行构建。

&emsp;&emsp;上述步骤在Windows 2000平台上MSVC7.1命令行版本测试通过，
没有尝试支持商用的Visual Studio 2003 IDE。

#### 2.2.2.4 Intel C++

&emsp;&emsp;本软件在Win32平台上，使用Intel C++编译器可以工作。
Intel C++需要微软Visual C++的头文件和lib文件，
所以你需要先安装Visual C++。

&emsp;&emsp;使用Intel C++的编译步骤
* 输入Intel C++环境的命令（你需要编辑并运行vcvar32.bat）。
* 在src文件夹输入```jam```来进行构建。

&emsp;&emsp;已经在MSVC 7.1命令行版本上Intel C++ 8.0编译器测试通过。
没有尝试支持Visual Studio 2003 IDE。

### 2.2.3 关于Fortran编译器
&emsp;&emsp;实现LBFGS核心算法的数值程序是用Fortran(lbfgs.f)实现的。
这意味着如果要在win32平台上使用此算法，你需要有Fortran编译器。
Intel Fortran 8.0编译器可以正常工作。你唯一需要做的工作是在文件Jamrules中的文件头部分添加一行```# USE_FORTRAN = 1;```

&emsp;&emsp;或者，你可以通过命令行手动编译lbfgs模块。调用```ifort -c -O3 lbfgs.f```会产生一个对象文件lbfgs.obj。

&emsp;&emsp;如果你没有Fortran编译器，LBFGS模块默认将不会被构建，这样只有GIS算法可以使用。

## 构建Python扩展
&emsp;&emsp;此工具包的很多功能来源于其python扩展模块。他融合了C++的运行速度以及Python的灵活度。python wrapper代码是基于[SWIG](#http://www.swig.org)产生的C++接口。
它比之前通过Boost.Pythonlib构建更加方便而且速度更快。
这一届











&emsp;&emsp;
