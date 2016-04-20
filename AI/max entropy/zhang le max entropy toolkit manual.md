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







&emsp;&emsp;
