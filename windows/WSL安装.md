## 安装WSL

### 启动Windows Subsystem for Linux

开始WSL之旅的第一步是`启动Windows Subsystem for Linux功能`，有两种方法实现：

##### 通过命令行

1.  以`管理员`身份打开`PowerShell`。

右键单击屏幕左下角`“开始”`菜单，找到“Windows PowerShell(管理员)”并打开

2.  复制以下命令并运行

```
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux
```

3.  安装完成后重启电脑

##### 通过控制面板

1.  打开控制面板

2.  找到启用或关闭Windows功能

    打开“程序”——“启用或关闭Windows功能”，找到并勾选“适用于Linux的Windows子系统”，点击“确定”。

3.  安装完成后重启电脑~

### 通过应用商店安装Linux发行版

##### 启动WSL支持之后，安装Linux发行版的步骤如下

1.  打开Win10[应用商店](https://www.microsoft.com/zh-cn/store/apps/)搜索WSL选择你喜欢的Linux发行版并安装。

    目前，WSL支持[Ubuntu](https://www.microsoft.com/zh-cn/store/p/ubuntu/9nblggh4msv6)，[Kali Linux](https://www.microsoft.com/zh-cn/store/p/kali-linux/9pkr34tncv07)，[GNU](https://www.microsoft.com/zh-cn/store/p/debian-gnu-linux/9msvkqc78pk6)，[OpenSUSE](https://www.microsoft.com/zh-cn/store/p/opensuse-leap-42/9njvjts82tjx)等发行版。**通过应用商店安装的Linux发行版只支持安装在`C`盘！**

2.  以安装`Ubuntu`为例，安装完成后搜索并打开`Ubuntu`执行后续安装。

3.  全部完成！今后我们可以通过`Ubuntu`应用打开一个WSL的Ubuntu命令行窗口。

## 升级到WSL2（确保BIOS开启VT）

1.  以`管理员`身份打开`PowerShell`。

右键单击屏幕左下角`“开始”`菜单，找到“Windows PowerShell(管理员)”并打开

2.  复制以下命令并运行

```powershell
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux
```

3.  安装完成后重启电脑

4.  以管理员身份打开powershell，输入`wsl -l`查看已经安装的子系统。

```powershell
PS C:\Windows\system32> wsl -l
适用于 Linux 的 Windows 子系统分发版:
Ubuntu-18.04 (默认)
PS C:\Windows\system32>
```

5.  输入命令`wsl --set-version Ubuntu-18.04 2`，这里的Ubuntu-18.04换成你的子系统名称

```powershell
C：\WINDOWS\system32> wsl —set-version Ubuntu-18.04 2 
Conversion in progress, this may take a few minutes...
For information on key differences with WSL 2 please visit https://aka.ms/wsl2
The distribution is already the requested version.
```

6.  输入`wsl -l -v`查看目前WSL版本，按下显示就是安装好了

```powershell
PS C:\Windows\system32> wsl -l -v
  NAME            STATE           VERSION
* Ubuntu-18.04    Stopped         2
```

## 安装桌面环境

### Ubuntu换源

这一步不是必须的

```bash
cp /etc/apt/sources.list /etc/apt/sources.list.bak
```

然后执行`vi /etc/apt/sources.list`并在文件最开始添加如下信息：

```bash
deb http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
```

### 更新与升级

```bash
sudo apt update
sudo apt upgrade
sudo apt autoremove
```

第三个命令不是必须的，只是为了清理用不到的包
另外如果遇到`bluemen`的报错，可以忽略不管。

### 安装桌面环境xubuntu

```bash
sudo apt install xubuntu-desktop
```

如果弹出选择gdm3或lightdm，选择gdm3

### 安装远程桌面服务xrdp

```bash
sudo apt install xrdp
```

### 配置xrdp端口

```bash
sudo sed -i 's/port=3389/port=3390/g' /etc/xrdp/xrdp.ini
```

这里面3389是默认配置，这里将其改为3390是避免和windows的端口冲突。子系统不是虚拟机，两边的端口号是通的。

### 配置xsession

```bash
sudo echo xfce4-session > ~/.xsession
```

### 重启一下电脑 不重启的时候会报错

### 启动xrdp

```bash
sudo service xrdp restart
```

启动成功后会看到如下提示

```bash
 * Starting Remote Desktop Protocol server 
[20190514-19:06:59] [DEBUG] Testing if xrdp can listen on 0.0.0.0 port 3390.
[20190514-19:06:59] [DEBUG] Closed socket 6 (AF_INET6 :: port 3390)[ OK ]
```

### 远程连接

在windows开始菜单中搜`远程桌面`打开后在`计算机`后面输入`localhost:3390`点击连接。首次连接可能需要较长时间。

## 终端工具链接

-   [Windows Terminal](https://www.microsoft.com/zh-cn/p/windows-terminal/9n0dx20hk701?rtc=1&activetab=pivot:overviewtab)

    