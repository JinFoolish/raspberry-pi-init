# 树莓派使用Ubuntu server从0开始搭建自己的系统环境

## 下载 :fa-download: 镜像启动

使用raspberry pi imager 烧录 [Ubuntu server](https://cn.ubuntu.com/download/raspberry-pi "Ubuntu server")镜像 即插即用。

## 配置 :fa-rss: WIFI

### CLI操作

- `sudo su` 进入root账号
- `ls /sys/class/net` 查看你的无线网络接口的名字叫啥，比如终端显示的是eth0 lo wlan0，无线网络接口名字就是wlan0。
- `ls /etc/netplan` 终端返回**50-cloud-init.yaml**，这就说明netplan文件叫**50-cloud-init.yaml**（可能有区别）。
- `vim /etc/netplan/50-cloud-init.yaml` 通过vim编辑器打开文件

### 配置文件

文件内容应该如下

```yaml
 network:
     ethernets:
        eth0:
            dhcp4: true
            optional: true
        version: 2
        wifis:
            wlan0:
                dhcp4: true
                optional: true
                access-points:
                    "WIFI NAME":
                    password: "PASS WORD"
```

应该需要写wifis及之后的内容
Esc退出编辑模式，输入`:wq`保存内容并退出

### 最后执行

- `netplan generate`
- `netplan apply`
看英文应该也能知道这两步操作做了什么

### 验证WIFI可用

`ping www.bilibili.com` ping一下咯 :fa-fighter-jet:
我就遇到了如下错误 **Temporary failure in name resolution**
于是执行`systemctl restart systemd-resolved.service` 后可以连接互联网
> 若是重启机器后又ping不通，将该命令加入开机启动项（应该不会遇到）

## 使用Zsh :fa-coffee: 让命令行更舒适

### 安装Zsh

1. `sudo apt-get install zsh`
2. 安装好后，使用`cat /etc/shells` 查看系统可以用的 shell
3. 使用 `chsh -s /bin/zsh`命令将 zsh 设置为系统默认 shell

------------

### 第一次运行 zsh 时进入配置引导页面

输入 **q** 会直接退出配置引导，下一次运行 zsh 时会再次进入配置引导。

输入**0**，也会退出配置引导，但是会在当前用户目录生成一个空白的文件*.zshrc*，下一次运行时就不会再进入配置引导。下一次运行时是否再进入配置引导，取决于用户目录下是否存在*.zshrc*文件。

输入输入 1 后，就开始进行配置

### 安装oh-my-zsh

1. git clone项目下来`git clone https://github.com/robbyrussell/oh-my-zsh.git ~/.oh-my-zsh`
2. `cp ~/.oh-my-zsh/templates/zshrc.zsh-template ~/.zshrc` 将oh-my-zsh项目的文件复制进zsh的配置当中
3. 完成:fa-flag:！

### 配置oh-my-zsh主题

编辑 ~/.zshrc 文件
`ZSH_THEME="robbyrussell"` 改变改行就可使用预设主题。
`ZSH_THEME_RANDOM_CANDIDATES=( "cloud" "jonathan" "apple")`
I Love :fa-heart: ！

#### 主题1：**agnoster**

[![agnoster](https://user-images.githubusercontent.com/49100982/108254745-777cb400-716c-11eb-800a-a8cfa612253f.jpg "agnoster")](https://user-images.githubusercontent.com/49100982/108254745-777cb400-716c-11eb-800a-a8cfa612253f.jpg "agnoster")

#### 主题2：**apple**

[![apple](https://user-images.githubusercontent.com/49100982/108254750-78ade100-716c-11eb-8c3b-7d529b7b4e25.jpg "apple")](https://user-images.githubusercontent.com/49100982/108254750-78ade100-716c-11eb-8c3b-7d529b7b4e25.jpg "apple")

#### 主题3： **cloud**

[![cloud](https://user-images.githubusercontent.com/49100982/108254774-7c416800-716c-11eb-9ea8-8f8cbac82922.jpg "cloud")](https://user-images.githubusercontent.com/49100982/108254774-7c416800-716c-11eb-9ea8-8f8cbac82922.jpg "cloud")

#### 主题4： **jonathan**

[![jonathan](https://user-images.githubusercontent.com/49100982/108254860-8c594780-716c-11eb-8f8b-be04d4943216.jpg "jonathan")](https://user-images.githubusercontent.com/49100982/108254860-8c594780-716c-11eb-8f8b-be04d4943216.jpg "jonathan")

### zsh插件

`plugins=(xxx yyy zzz)` 在**.zshrc**文件中修改该行使用插件。
所有插件应该在 .../.oh-my-zsh/custom/plugins 目录下

#### [zsh-syntax-highlighting](https://github.com/zsh-users/zsh-syntax-highlighting "zsh-syntax-highlighting")

`git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions`
将命令的颜色做区分，正确的命令会显示绿色，错误的显示红色，看上去就舒服很多

#### [zsh-autosuggestions](https://github.com/zsh-users/zsh-autosuggestions "zsh-autosuggestions")

`git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions`
可以根据以往命令历史，自动补全命令

`ZSH_AUTOSUGGEST_HIGHLIGHT_STYLE="fg=#ff00ff,bg=cyan,bold,underline"`
该设置可以调整自动提示字体颜色等配置
可以支持的颜色有限：
> black , red , green , yellow , blue , magenta , cyan, white

## 调整字体

**字体**`sudo dpkg-reconfigure console-setup`  进入设置界面

## 美化图形化界面

### 安装i3wm

i3wm 不同于其它窗口管理器，它没有固定的平铺模式（不会事先安排如何切割桌面来铺窗口），所以它能更灵活的安排自己的工作，窗口比例。

1. 由于没有图形化界面，我们需要先安装 `sudo apt install xinit`
2. 再安装i3wm `sudo apt install i3`
3. `startx`启动图形界面进入i3

### 配置i3

> 配置文件路径~/.config/i3/config

- 配置透明终端 `sudo apt install compton`
    在配置文件中加入**exec_always --no-startup-id compton**

- 安装终端 `sudo apt install terminator`
    在配置文件中改变**bindsym $mod+Return exec alacritty**

- 配置terminator

- 在Ubuntu上安装 i3-gaps
安装*依赖项*

> sudo apt install libxcb1-dev libxcb-keysyms1-dev libpango1.0-dev libxcb-util0-dev libxcb-icccm4-dev libyajl-dev libstartup-notification0-dev libxcb-randr0-dev libev-dev libxcb-cursor-dev libxcb-xinerama0-dev libxcb-xkb-dev libxkbcommon-dev libxkbcommon-x11-dev autoconf xutils-dev libtool

```shell
cd /tmp
git clone https://www.github.com/Airblader/i3 i3-gaps
cd i3-gaps
git checkout gaps && git pull
autoreconf --force --install
rm -rf build
mkdir build
cd build
../configure --prefix=/usr --sysconfdir=/etc
make
sudo make install
```

### i3快捷键

``` shell
$mod + Enter    启动虚拟终端
$mod + A    焦点转义到父窗口上
$mod + S    堆叠布局
$mod + W    标签布局
$mod + E    默认布局
$mod + SpaceBar 焦点在平铺式／浮动式转换
$mod + D    启动 dmenu
$mod + H    水平分割窗口
$mod + V    垂直分割窗口
$mod + J    焦点往左窗口移
$mod + K    焦点往下窗口移
$mod + L    焦点往上窗口移
$mod + ;    焦点往右窗口移
$mod + Shift + Q    杀死当前窗口的进程
$mod + Shift + E    退出 i3
$mod + Shift + C    当场重新加载 i3config, 无需重启
$mod + Shift + R    重启 i3 （还重新加载了 i3config, 又没有退出过程）
$mod + Shift + J    窗口左移
$mod + Shift + K    窗口下移
$mod + Shift + L    窗口上移
$mod + Shift + :    窗口右移
$mod + Shift + SpaceBar 窗口在平铺式／浮动式转换
$mod  + <数字键> 切换window
$mod + shift + <数字键> 将当前程序发送到指定window
$mod + F 全屏
```

### 连接蓝牙设备

- 开启蓝牙 `sudo service bluetooth start`

- 进入bluetoothctl `bluetoothctl`

- 输入以下命令

```shell
power on 
agent on 
default-agent 
scan on 
pair yourDeviceMAC
```

匹配成功！
