# 软件包管理

各位小伙伴大家好，咱们接着前面的课程，继续讲解 GitHub 开源之旅第九季：Linux Bash 入门，现在咱们讲解第三个话题：携手同行之软件包管理。

我们在选择 Linux 发行版本时，通常有一个重要的指标，什么指标呢？软件包管理。这个问题稍微展开一下，涉及到三个方面：第一，这个发行版是否有好用的软件包管理系统，第二，这个发行版是否有丰富的软件包供我们使用，第三，这些丰富的软件包是否有持久的社区维护更新。如果我们多少有些 Linux 的使用经验，我们会发现 Linux 上的软件包更新很快。大多数一线 Linux 发行版每隔半年会发布一个新版本，并且许多独立的程序每天都会更新。为了能很好的掌握和利用这些多如牛毛的软件，我们需要软件包管理工具来帮助我们完成这个工作。

软件包管理是指系统中一种安装和维护软件的方法。现在，Linux 发行版中的软件包，已经能够满足大部分人的日常需要。而对于早期的 Linux 用户，人们通常需要下载和编译源码来安装软件。编译源码没有任何问题，事实上，拥有对源码的访问权限是 Linux 的伟大之处。它赋予我们每个人定制和优化系统的权利。只是如果有一个预先编译好的软件包，使用起来会相对容易和快速一些。本次课程，我们将介绍一些软件包管理方面的知识。

## 打包系统

不同的 Linux 发行版使用不同的打包系统，一般而言，大多数发行版分别属于两大包管理技术阵营：Debian 的".deb"，和红帽的".rpm"。当然，也有一些例外，比方说 Gentoo，Slackware，和 Foresight，但大多数会使用这两个基本系统中的一个。因为我们教学的环境选择的是 CentOS，所以我们只介绍 CentOS 的包管理方式，CentOS 是 RedHat 的社区版，所以使用的包管理方式跟红帽的相同。

<table class="multi">
<caption class="cap">表15-1: 主要的包管理系统家族</caption>
<tr>
<th class="title">包管理系统</th>
<th class="title">发行版 (部分列表)</th>
</tr>
<tr>
<td valign="top" width="25%">Debian Style (.deb) </td>
<td valign="top">Debian, Ubuntu, Xandros, Linspire</td>
</tr>
<tr>
<td valign="top">Red Hat Style (.rpm) </td>
<td valign="top">Fedora, CentOS, Red Hat Enterprise Linux, OpenSUSE, Mandriva, PCLinuxOS</td>
</tr>
</table>

## 软件包管理系统是怎样工作的

在商业化的软件产业中，软件发布的方法通常需要购买一张安装光盘，然后运行"安装向导"，在系统中安装新的应用程序。

Linux 不是这样。Linux 系统中几乎所有的软件都可以在互联网上找到。其中大多数软件由发行商以包文件的形式提供，剩下的则以源码形式存在，可以手动安装。我们在下篇课程的最后，会介绍如何通过编译源码来安装软件。

## 包文件

在包管理系统中软件的基本单元是包文件。包文件是一个压缩文件，这个压缩文件包含构成软件包的所有文件集合。一个软件包可能由大量程序以及支持这些程序的数据文件组成。除了安装文件之外，软件包文件也包括关于这个包的元数据，例如：软件包及其内容的文本说明。另外，许多软件包还包括预安装或安装后脚本，这些脚本用来在软件安装之前和之后执行配置任务。

软件包文件是由软件包维护者创建的，他通常是软件的开发人员，但是也不绝对。软件维护者从程序员那里得到软件源码，然后编辑源码，创建软件包元数据以及所需要的安装脚本。

## 资源库

虽然某些软件项目选择执行他们自己的打包和发布策略，但是现在大多数软件包是由发行商和感兴趣的第三方创建的。系统发行版的用户可以在一个中心资源库中得到这些软件包，这个资源库可能包含了成千上万个软件包，每一个软件包都是专门为这个系统发行版建立和维护的。

因软件开发生命周期不同阶段的需要，一个系统发行版可能维护着几个不同的资源库。例如，通常会有一个"测试"资源库，其中包含刚刚建立的软件包，它们想要勇敢的用户来使用，在这些软件包正式发布之前，让用户查找错误。系统发行版经常会有一个"开发"资源库，这个资源库中保存着注定要包含到下一个主要版本中的半成品软件包。

## 依赖性

程序很少是"孤立的"，而是依赖于其它软件组件来完成它们的工作。以输入/输出为例，就是由共享程序例程来处理的。这些程序例程存储在共享库中，共享库不只为一个程序提供基本服务。如果一个软件包需要共享资源，我们说这就存在一个依赖。现代的软件包管理系统都提供了一些依赖项解析方法，以此来确保当安装软件包时，也安装了其所有的依赖程序。

## 上层和底层软件包工具

软件包管理系统通常由两种工具类型组成：底层工具用来处理一些任务，比方说安装和删除软件包文件，而上层工具，完成元数据搜索和依赖解析。Debian 和 Red Hat 使用的底层和上层软件包工具各不相同，我们主要介绍 Red Hat 软件包工具。而且我们只讲上层工具 yum 的用法，不讲 rpm 命令的用法。

<table class="multi">
<caption class="cap">表15-2: 包管理工具</caption>
<tr>
<th class="title">发行版</th>
<th class="title">底层工具</th>
<th class="title">上层工具</th>
</tr>
<tr>
<td valign="top">Debian-Style</td>
<td valign="top">dpkg</td>
<td valign="top">apt-get, aptitude</td>
</tr>
<tr>
<td valign="top">Fedora, Red Hat Enterprise Linux, CentOS</td>
<td valign="top">rpm</td>
<td valign="top">yum</td>
</tr>
</table>

## 常见软件包管理任务

通过命令行软件包管理工具可以完成许多常规的软件包管理任务。底层工具也支持软件包文件的创建，如何创建软件包不在我们课程讨论范围之内。

## 查找资源库中的软件包

使用上层工具来搜索资源库元数据，可以根据软件包的名字和说明来定位它。

例如：搜索一个 yum 资源库来查找 vim 文本编辑器，使用以下命令：

    yum search vim

## 列出所安装的软件包

列出已安装的和可安装的应用程序包。

    yum list
    yum list | less
    @anaconda 和 @base 代表已经安装的软件包，在前面列出

    如果只想显示所有已经安装的软件包
    yum list installed
    
    如果只想查看某个软件包的信息
    yum list git

## 确定是否安装了一个软件包

 通过 yum list 命令结合 grep 查找命令，我们可以判断是否安装了某个软件包。

例如：确定是否 Debian 风格的系统中安装了这个 emacs 软件包：

    yum list | grep git
    yum list installed | grep git

## 从资源库中安装一个软件包

上层工具允许从一个资源库中下载一个软件包，并经过完全依赖解析来安装它。

例如：从一个 yum 资源库来安装 git 版本控制系统：

    sudo yum install -y git
    git --version

## 卸载软件

可以使用上层或者底层工具来卸载软件。下面是可用的上层工具。

例如：yum 卸载 git 软件包：

    sudo yum erase git

## 经过资源库来更新软件包

最常见的软件包管理任务是保持系统中的软件包都是最新的。上层工具仅需一步就能完成这个至关重要的任务。

例如：更新安装在 Debian 风格系统中的软件包：

    yum list updates 列出所有可以升级的软件包
    yum update package 升级 package

    sudo yum update yum

## 显示所安装软件包的信息

如果知道了所安装软件包的名字，使用以下命令可以显示这个软件包的说明信息：

例如：查看 redhat 风格的系统中 git 软件包的说明信息：

    yum info git
    yum info net-tools

## 总结归纳

好，本小节软件包管理的内容我们就讲这么多，下面咱们一块看一下通关任务。请看完本次课程视频的小伙伴们完成一下通关任务。