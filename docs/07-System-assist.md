# 系统辅助 {docsify-ignore}

## 10.1 Oracle VM VirtualBox

### 10.1.1 扩展虚拟硬盘容量

> `Oracle VM VirtualBox` 管理器没有提供可视化的方式修改硬盘容量，所以只能通过命令行的方式进行修改。

解决方案：

```powershell
PS C:\WINDOWS\system32> cd "C:\Program Files\Oracle\VirtualBox"
PS C:\Program Files\Oracle\VirtualBox>
PS C:\Program Files\Oracle\VirtualBox> .\VBoxManage list hdds
UUID:           f016e1d2-f88b-4942-9cc0-08a166e92324
Parent UUID:    base
State:          created
Type:           normal (base)
Location:       G:\VirtualBox VMs\Windows XP\Windows XP.vdi
Storage format: VDI
Capacity:       10240 MBytes
Encryption:     disabled
PS C:\Program Files\Oracle\VirtualBox> .\VBoxManage modifyhd f016e1d2-f88b-4942-9cc0-08a166e92324 --resize 11264
0%...10%...20%...30%...40%...50%...60%...70%...80%...90%...100%
```

> 修改硬盘容量可以通过 `Parent UUID` 或者 `Location`，两者任选其一。

!> 如果使用 `Location` 作为修改方式，路径中有空格或中文用 `""` 引起来，代码如下：

```powershell
PS C:\Program Files\Oracle\VirtualBox> .\VBoxManage modifyhd "G:\VirtualBox VMs\Windows XP\Windows XP.vdi" --resize 11264
0%...10%...20%...30%...40%...50%...60%...70%...80%...90%...100%
```

### 10.1.2 虚拟机无法启动

> 最近装了 `Windos 10 Linux 子系统`，导致和 `Oracle VM VirtualBox` 不兼容，所以禁用 `Hyper-V` 就可以正常运行虚拟机了。

解决方案：

```powershell
PS C:\WINDOWS\system32> bcdedit /set hypervisorlaunchtype off
操作成功完成。
```

> 如果运行虚拟机还是不能正常启动，请重启电脑。

### 10.1.3 安装 macOS Big Sur 11.0 Beta

> 之前一直用的 `VMware WorkStation`，听说 `Oracle VM VirtualBox` 比较轻。最近苹果的新系统出来了，在虚拟机安装体验一下。

一、环境准备：

* Windows 10：`20H2`
* Oracle VM VirtualBox：`6.1.10`
* macOS：`Big Sur Developer Beta.cdr`

二、安装步骤

1. `新建` → `专家模式`，设置虚拟机 `名称`、`文件夹`、`内存`（建议 4 GB 以上），选择 `现在创建虚拟硬盘`，点击 `创建`。

  ![B18](../images/B18.jpg)

2. `文件大小` 至少 `50 GB`，点击 `创建`。

  ![B19](../images/B19.png)

3. 取消 `软驱` 的勾选。

  ![B20](../images/B20.png)

4. `处理器数量` 输入 `2`。

  ![B21](../images/B21.png)

5. `显存大小` 改为 `128`，勾选 `启用3D加速`。

  ![B22](../images/B22.png)

6. 选中 `没有盘片`，点击 `分配光驱` 后面的图标，点击 `选择虚拟盘`，找到并 `打开` 系统镜像文件，最后点击 `OK`。

  ![B23](../images/B23.png)

7. 关闭 `VirtualBox`，打开 `Wndows PowerShell`，输入如下命令：

  ```powershell
  PS C:\Users\89349> cd "C:\Program Files\Oracle\VirtualBox\"
  PS C:\Program Files\Oracle\VirtualBox> VBoxManage modifyvm "Mac OS" --cpuid-set 00000001 000106e5 00100800 0098e3fd bfebfbff
  PS C:\Program Files\Oracle\VirtualBox> .\VBoxManage modifyvm "Mac OS" --cpuid-set 00000001 000106e5 00100800 0098e3fd bfebfbff
  PS C:\Program Files\Oracle\VirtualBox> .\VBoxManage setextradata "Mac OS" "VBoxInternal/Devices/efi/0/Config/DmiSystemProduct" "iMac19,1"
  PS C:\Program Files\Oracle\VirtualBox> .\VBoxManage setextradata "Mac OS" "VBoxInternal/Devices/efi/0/Config/DmiSystemVersion" "1.0"
  PS C:\Program Files\Oracle\VirtualBox> .\VBoxManage setextradata "Mac OS" "VBoxInternal/Devices/efi/0/Config/DmiBoardProduct" "Mac-AA95B1DDAB278B95"
  PS C:\Program Files\Oracle\VirtualBox> .\VBoxManage setextradata "Mac OS" "VBoxInternal/Devices/smc/0/Config/DeviceKey" "ourhardworkbythesewordsguardedpleasedontsteal(c)  AppleComputerInc"
  PS C:\Program Files\Oracle\VirtualBox> .\VBoxManage setextradata "Mac OS" "VBoxInternal/Devices/smc/0/Config/GetKeyFromRealSMC" 1
  PS C:\Program Files\Oracle\VirtualBox> .\VBoxManage setextradata "Mac OS" VBoxInternal2/EfiGraphicsResolution 1920x1080
  ```

  > 1. 最后一条命令是设置虚拟机系统的分辨率大小，分辨率大小可根据实际情况进行调整。  
  > 2. 命令中的 `Mac OS` 是虚拟机的名称。每条命令执行后，没有任何提示，则表示成功；否则表示失败。

8. 如果没有安装过其它虚拟机系统，默认就是 `macOS` 的镜像，点击 `启动`。

  ![B24](../images/B24.png)

9. 启动后，显示如下界面，等待执行完成。

  ![B25](../images/B25.png)

10. 进入安装界面，选择 `语言`，点击 `→`。

  ![B26](../images/B26.png)

11. 选择 `磁盘工具`，点击 `继续`。

  ![B27](../images/B27.png)

12. 选择第一个磁盘（VBOX HARDDISK Media），点击 `抹掉`，输入磁盘 `名称`，然后再点击 `抹掉`。

  ![B28](../images/B28.png)

13. 关闭 `磁盘工具`，选择 `安装macOS`，点击 `继续`。

  ![B29](../images/B29.png)

14. 点击 `同意` 协议。

  ![B30](../images/B30.jpg)

15. 选中 `Mac OS` 磁盘，点击 `安装`。

  ![B31](../images/B31.png)

16. 进度走完后，系统会重新启动，紧接着继续安装系统。

  ![B32](../images/B32.png)

  ![B33](../images/B33.png)

17. 等待安装进度完成（这个过程挺漫长，其实安装过程一直在进行，虚拟机系统文件一直在刷新 `大小` 和 `修改时间`）。

  ![B56](../images/B56.png)

18. `选择国家或地区`、`语言与输入法`、`数据与隐私`，点击 `继续`；`辅助功能`、`迁移助理`，点击 `以后`。

  ![B38](../images/B38.jpg)

  ![B39](../images/B39.jpg)

  ![B40](../images/B40.jpg)

  ![B41](../images/B41.jpg)

  ![B42](../images/B42.jpg)

19. `跳过` 登录，点击 `稍后设置`。

  ![B43](../images/B43.jpg)

20. `条款与条件`，点击`同意`。

  ![B44](../images/B44.jpg)

21. 输入 `全名`、`密码`（`账户名称` 会以 `全名` 小写形式带出），点击 `继续`。

  ![B45](../images/B45.jpg)

22. 连续点击`继续`。

  ![B46](../images/B46.png)

  ![B47](../images/B47.jpg)

  ![B48](../images/B48.jpg)

  ![B49](../images/B49.jpg)

23. 根据提示按相应的 `键`，然后点击  `完成`。

  ![B50](../images/B50.jpg)

  ![B51](../images/B51.jpg)

  ![B52](../images/B52.jpg)

  ![B53](../images/B53.jpg)

24. 点击 `🍎` 图标，选择 `关机`。

  ![B54](../images/B54.jpg)

25. 点击 `设置`，选中 `光驱`，点击 `光驱图标`，选择 `移除虚拟盘`。

  ![B55](../images/B55.png)

> 苹果虚拟机使用欠佳，有能力上黑苹果，有 `money` 上白苹果。

### 10.1.4 虚拟系统克隆

1. 首先进入 `VirtualBox` 的安装路径，打开 `powershell`，输入 `.\VBoxManage clonehd`，查看克隆命令相关参数。

  ```powershell
  PS C:\Program Files\Oracle\VirtualBox> .\VBoxManage clonehd
  VBoxManage clonemedium      [disk|dvd|floppy] <uuid|inputfile> <uuid|outputfile>
                              [--format VDI|VMDK|VHD|RAW|<other>]
                              [--variant Standard,Fixed,Split2G,Stream,ESX]
                              [--existing]
  ```

  > 本次操作以文件参数为例，其他参数不做叙述。

2. 输入 `.\VBoxManage clonehd 源虚拟系统文件地址 目标虚拟系统文件地址`，执行命令中加双引号是因为目录有空格，不加执行报错。执行完成后会返回一个 `UUID`，这是一个虚拟系统的唯一标志。

  ```powershell
  PS C:\Program Files\Oracle\VirtualBox> .\VBoxManage clonehd "G:\VirtualBox VMs\CentOS 7.8\CentOS 7.8-disk001.vdi" "G:\VirtualBox VMs\CentOS 7.8-Mirrors-Test\CentOS 7.  8-Mirrors-Test-disk001.vdi"
  0%...10%...20%...30%...40%...50%...60%...70%...80%...90%...100%
  Clone medium created in format 'VDI'. UUID: 636f8e4b-2a04-4940-a301-5030930c4832
  ```

3. `Oracle VM VirtualBox` 软件中点击 `新建`，然后输入虚拟系统名称，选择系统类型、版本，接着选择克隆命令生成的虚拟系统文件，点击 `创建`，结束。

  ![B211](../images/B211.png)

  !> 不要提前在目标目录下创建好和虚拟系统名称一致的同名文件夹，否则会提示文件夹已存在，不让创建虚拟机。

### 10.1.5 Could not mount the media/drive 'C:\Program Files\Oracle\VirtualBox\VBoxGuestAdditions.iso' (VERR_PDM_MEDIA_LOCKED).

> 参考 [简书](https://www.jianshu.com/p/da1bdc673f2e) 解决此问题。

`VirtualBox` 安装 `Deepin 20` 后，显示中没有 `1920*1080` 的分别率。

![B229](../images/B229.png)

安装增强工具提示错误。

![B230](../images/B230.png)

终端中输入如下命令，执行完成后，重启 `Deepin 20` 。

```bash
jack@jack-PC:~$ sudo mkdir --P /media/cdrom
jack@jack-PC:~$ sudo mount -t auto /dev/cdrom /media/ cdrom/
jack@jack-PC:~$ cd /media/cdrom
jack@jack-PC:/media/cdrom$ sudo sh VBoxLinuxAdditions.run
```
![B231](../images/B231.png)

> `VirtualBox` 菜单中选择全屏模式，可以在显示中看到 `1920*1080` 的分别率，退出后，不显示，但显示效果是 `1920*1080` 的分别率。