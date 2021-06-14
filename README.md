# 持续更新中 每周五

本仓库适用于各国内CSGO各开服服主，收录了各类开服需要用到的插件，同时提供Linux端的保姆级开服教程，对于插件和教程的问题，欢迎提交issue。



## 安装前的注意事项

>1.如果是标注为内核汉化的插件，此类插件没有写入外部汉化接口，后续的更改请在.sp源码文件中进行，需要安装sourcemod进行重新的编译。
>
>如果是标注为外部汉化的插件，说明sourcemod可以根据json格式来辨别系统使用的语言来为玩家提供不同的显示语言
>
>同时，外部汉化插件只提供其外部汉化文件，源文件请到作者原仓库进行下载。

> 2.其中一些插件作者已经停止维护插件，某些问题本仓库已经修复并重新编译，不能保证所有插件都修复了历史遗留bug

> 3.某些插件写入了内外网的命令，标记为LAN的插件是内网专用，请仔细辨别以免影响开服流程

## 如何安装汉化

>内核汉化文件请将smx拖入addons/sourcemod/plugins文件夹下
>
>外部汉化文件请在原有smx已经安装的情况下将外部汉化文件拖入addons/sourcemod/translations下
>
>注意：如果插件原有的语言文件下已经有各国语言翻译，可以考虑用编辑器合并

# 外网如何使用Linux平台架设服务器(Source Dedicated Server)

>获取一台云服务器（各大运营商）
>
>建议新手选择Ubuntu镜像，本教程将以Ubuntu 20.04作为模板，各发行版略有不同

```sh
# 首先要更新你的系统

sudo apt update
sudo apt upgrade

# 安装依赖

sudo dpkg --add-architecture i386; sudo apt update; sudo apt install curl wget file tar bzip2 gzip unzip bsdmainutils python util-linux ca-certificates binutils bc jq tmux netcat lib32gcc1 lib32stdc++6 libsdl2-2.0-0:i386 steamcmd

# 创建一个新的用户csgoserver，当然也可以是其他的
adduser csgoserver

# 切换到csgoserver用户及其目录下
su - csgoserver

# 获取linux game server manager脚本并赋予其执行权限并执行，如果中途出错可以拆分分步执行
wget -O linuxgsm.sh https://linuxgsm.sh && chmod +x linuxgsm.sh && bash linuxgsm.sh csgoserver

# 使用该目录下的csgoserver执行程序执行安装，这一步执行完后会要求你填入token，下一步解释
./csgoserver install

# 你需要到 Steam 游戏服务器帐户管理页面进行 服务器令牌 申请操作，获取服务器令牌/token
# 浏览器访问
# https://steamcommunity.com/dev/managegameservers

# 如果你恰逢/刚好/碰巧 跳过了填入token的一步
vim lgsm/config-lgsm/csgoserver/csgoserver.cfg
# 将token填入 gslt=""中

# 在服务器运营商控制面板的防火墙页面，将TCP/UDP 27005与27015 端口开放，这是默认的两个端口

# 开启服务器
./csgoserver start

# 关闭服务器
./csgoserver stop

# 关闭服务器
./csgoserver restart

# 进入服务器控制台
./csgoserver console

# 服务器的手动更新
./csgoserver update

# 权限 --待更新
```

## 安装sourcemod

> sourcemod是一切插件的基础环境，请在服务器安装好后着手安装
>
> 1. 前往sourcemod官网[sourcemod](https://www.sourcemod.net/downloads.php?branch=stable)下载sourcemod Linux版(stable builds)
>
> 2. 前往metamod官网[metamod](https://www.sourcemm.net/downloads.php/?branch=stable) 下载metamod Linux版(stable builds)
>
>    
>
>    
>
>    
>
>    



# 躲猫猫(prophunt) 

源仓库 [prophunt](https://github.com/olavim/sm-PropHunt) 

> 该插件作者已经停止更新，个人外部汉化。
>
> > 针对CT蹲下鬼畜的问题，可以将插件设置中的1.5倍速调为正常的1倍速。

# 满十比赛服(smp)

源帖 [Simple match plugin for CS:GO](https://forums.alliedmods.net/showthread.php?p=2404215)

> 该插件作者已经停止更新，个人内核汉化

# 多人1V1练枪服务器(multi1v1)

源仓库 [csgo-multi-1v1](https://github.com/splewis/csgo-multi-1v1)

> 该插件自带多国语言翻译，请移步至源仓库

# 皮肤插件(playerskin)

源仓库 [playerskin](https://github.com/safra36/PlayerSkin)

> 该插件自带多国语言翻译，请移步至源仓库

