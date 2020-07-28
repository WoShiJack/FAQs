## 2.1 &nbsp;&nbsp;Linux

### 2.1.1 &nbsp;&nbsp;CentOS

#### 2.1.1.1 &nbsp;&nbsp;CentOS 7 清空终端屏幕

解决方案：

* 输出很多信息的命令行窗口：

![A7](../images/A7.png)

* 执行命令：

```bash
root@192.168.1.10:~# reset
```

* 清空后的窗口:

![A8](../images/A8.png)

#### 2.1.1.2 &nbsp;&nbsp;CentOS 7 删除多余的内核

解决方案：

1. 首先列出系统中正在使用的内核。

```bash
uname -a
```

2. 查询系统中全部的内核。

```bash
rpm -qa | grep kernel
```

3. 将需要删除的内核删除掉（多个内核可空格隔开，也可以写模糊的内核名称）。

```bash
yum -y remove 内核名称
```

4. 重启之后就可以看到内核被删掉了，同时多余的启动项也自动删掉了。

#### 2.1.1.3 &nbsp;&nbsp;CentOS 7 中安装或升级最新的内核

> 本文转载自[Linux 中国](https://linux.cn/article-8310-1.html)

虽然有些人使用 `Linux` 来表示整个操作系统，但要注意的是，严格地来说，`Linux` 只是个内核。另一方面，发行版是一个完整功能的系统，它建立在内核之上，具有各种各样的应用程序工具和库。

在正常操作期间，内核负责执行两个重要任务：

* 作为硬件和系统上运行的软件之间的接口。

* 尽可能高效地管理系统资源。

为此，内核通过内置的驱动程序或以后可作为模块安装的驱动程序与硬件通信。

例如，当你计算机上运行的程序想要连接到无线网络时，它会将该请求提交给内核，后者又会使用正确的驱动程序连接到网络。

> 建议阅读： [如何在 Ubuntu 中升级内核](https://linux.cn/article-8284-1.html)

随着新的设备和技术定期出来，如果我们想充分利用它们，保持最新的内核就很重要。此外，更新内核将帮助我们利用新的内核函数，并保护自己免受先前版本中发现的漏洞的攻击。

准备好了在 `CentOS 7` 或其衍生产品（如 `RHEL 7` 和 `Fedora`）上更新内核了么？如果是这样，请继续阅读！

1. 检查已安装的内核版本。

让我们安装了一个发行版，它包含了一个特定版本的内核。为了展示当前系统中已安装的版本，我们可以：

```bash
uname -sr
```

下面的图片展示了在一台 `CentOS 7` 服务器上的输出信息：

![A9](../images/A9.png)

如果我们现在进入 [Linux 内核官网](https://www.kernel.org/)，在撰写本文时，我们看到最新的内核版本是 `4.10.1`（其他版本可以从同一网站获得）。

还要考虑的一个重要的事情是内核版本的生命周期 - 如果你当前使用的版本接近它的生命周期结束，那么在该日期后将不会提供更多的 `bug` 修复。关于更多信息，请参阅[内核发布页](https://www.kernel.org/category/releases.html)。

2. 在 `CentOS 7` 中升级内核。

大多数现代发行版提供了一种使用 [yum等包管理系统](http://www.tecmint.com/20-linux-yum-yellowdog-updater-modified-commands-for-package-mangement/) 和官方支持的仓库升级内核的方法。

但是，这只会升级内核到仓库中可用的最新版本 - 而不是在 [Linux 内核官网](https://www.kernel.org/) 中可用的最新版本。不幸的是，`Red Hat` 只允许使用前者升级内核。

与 `Red Hat` 不同，`CentOS` 允许使用 `ELRepo`，这是一个第三方仓库，可以将内核升级到最新版本。

要在 `CentOS 7` 上启用 `ELRepo` 仓库，请运行：

```bash
rpm --import https://www.elrepo.org/RPM-GPG-KEY-elrepo.org
rpm -Uvh http://www.elrepo.org/elrepo-release-7.0-2.el7.elrepo.noarch.rpm
```

![A10](../images/A10.png)

仓库启用后，你可以使用下面的命令列出可用的内核相关包：

```bash
yum --disablerepo="*" --enablerepo="elrepo-kernel" list available
```

![A11](../images/A11.png)

接下来，安装最新的主线稳定内核：

```bash
yum --enablerepo=elrepo-kernel install kernel-ml
```

![A12](../images/A12.png)

最后，重启机器并应用最新内核，接着运行下面的命令检查最新内核版本：

```bash
uname -sr
```

![A13](../images/A13.png)

3. 设置 `GRUB` 默认的内核版本。

为了让新安装的内核成为默认启动选项，你需要如下修改 GRUB 配置：

打开并编辑 `/etc/default/grub` 并设置 `GRUB_DEFAULT=0`。意思是 `GRUB` 初始化页面的第一个内核将作为默认内核。

```text
GRUB_TIMEOUT=5
GRUB_DISTRIBUTOR="$(sed 's, release .*$,,g' /etc/system-release)"
GRUB_DEFAULT=0
GRUB_DISABLE_SUBMENU=true
GRUB_TERMINAL_OUTPUT="console"
GRUB_CMDLINE_LINUX="crashkernel=auto rd.lvm.lv=centos/root rd.lvm.lv=centos/swap rhgb quiet"
GRUB_DISABLE_RECOVERY="true"
```

接下来运行下面的命令来重新创建内核配置。

```bash
grub2-mkconfig -o /boot/grub2/grub.cfg
```

![A14](../images/A14.png)

重启并验证最新的内核已作为默认内核。

![A15](../images/A15.png)

恭喜你！你已经在 `CentOS 7` 中升级内核了！

4. 总结

本文中，我们解释了如何轻松升级系统上的 `Linux` 内核。我们还没讲到另外一个方法，因为它涉及从源代码编译内核，这可以写成一本书，并且不推荐在生产系统上这么做。

虽然它是最好的学习体验之一，并且允许细粒度配置内核，但是你可能会让你的系统不可用，并且可能必须从头重新安装它。

如果你仍然有兴趣构建内核作为学习经验，你可以在 [Kernel Newbies](https://kernelnewbies.org/KernelBuild) 页面中获得指导。

一如既往，如果你对本文有任何问题或意见，请随时使用下面的评论栏。

作者简介：

我是一个计算机上瘾的家伙，并且是开源和 `Linux` 系统软件的粉丝，有大约 4 年的 `Linux` 发行版桌面、服务器和 `bash` 脚本的经验。

[via](http://www.tecmint.com/install-upgrade-kernel-version-in-centos-7/)

作者：`Matei Cezar` 译者：`geekpi` 校对：`wxy`

本文由 `LCTT` 原创编译，`Linux中国` 荣誉推出