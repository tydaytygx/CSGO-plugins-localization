# 各教程持续更新中 每周N/A

本仓库适用于各国内CSGO各开服服主，收录了各类开服需要用到的插件，同时提供Linux端的保姆级开服教程，对于插件和教程的问题，欢迎提交issue。

[![GitHub license](https://img.shields.io/github/license/tydaytygx/CSGO-plugins-localization)](https://github.com/tydaytygx/CSGO-plugins-localization/blob/main/LICENSE)[![GitHub stars](https://img.shields.io/github/stars/tydaytygx/CSGO-plugins-localization)](https://github.com/tydaytygx/CSGO-plugins-localization/stargazers)[![GitHub issues](https://img.shields.io/github/issues/tydaytygx/CSGO-plugins-localization)](https://github.com/tydaytygx/CSGO-plugins-localization/issues)







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
## 请在防火墙开启端口UDP 27015 27005（默认端口）否则客户端无法接入

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

# 自动定时更新
crontab -e
*/5 * * * * /home/csgoserver/csgoserver monitor > /dev/null 2>&1
*/30 * * * * /home/csgoserver/csgoserver update > /dev/null 2>&1
0 0 * * 0 /home/csgoserver/csgoserver update-lgsm > /dev/null 2>&1
```

## 安装sourcemod

> sourcemod与metamod是插件的基础环境，请在服务器安装好后着手安装
>
> 前往sourcemod官网[sourcemod](https://www.sourcemod.net/downloads.php?branch=stable)下载sourcemod Linux版(stable builds)
>
> 前往metamod官网[metamod](https://www.sourcemm.net/downloads.php/?branch=stable) 下载metamod Linux版(stable builds)
## 资源从本地推送到服务器(sftp)

> 强烈建议使用带图形化sftp的命令行工具
>
> 例如
>
> > mobaxterm

```sh
# 不使用图形界面进行推送安装包
sftp csgoserver@服务器ip/域名
put 包名
# 直接在命令行下载
# 注意 以下的命令请以官方下载的本版号为准，解压的时候注意包名
wget https://sm.alliedmods.net/smdrop/1.10/sourcemod-1.10.0-git6503-linux.tar.gz
wget https://mms.alliedmods.net/mmsdrop/1.11/mmsource-1.11.0-git1144-linux.tar.gz
```
# 假如你已经把包放到了服务器上(csgoserver用户目录下) 图形/非图形界面
## 如果提示权限不足无法解压 可能是上传时没有使用正确的用户连接sftp sftp用户与创建session时的用户一致
```sh
tar zxvf sourcemod-1.10.0-git6503-linux.tar.gz -C serverfiles/csgo/
tar zxvf mmsource-1.11.0-git1144-linux.tar.gz -C serverfiles/csgo/
```
# 两个文件夹堆叠起来后即完成sourcemod环境的安装 位于 serverfiles/csgo/
# 重启服务器
./csgoserver restart


## 本格的插件安装
+ 将编译好的插件本体 ```.smx``` 文件放入目录
> serverfiles/csgo/addons/sourcemod/plugins
+ 其中 某些插件的设置会在 这两个目录下
> serverfiles/csgo/addons/sourcemod/configs
> serverfiles/csgo/cfg/sourcemod
+ 插件的设置改变不是实时的 你可以使用全局卸载并重新加载的姿势
```sh
sm plugins unload_all
sm plugins refresh
```
+ 或者 对单个插件进行重载
```sh
sm plugins reload 插件名
```
# 你知道吗 服务器全局设置位于 serverfiles/csgo/cfg/csgoserver.cfg
### Sourcemod权限部分

首先前往[steamidfinder](https://www.steamidfinder.com/)

获取你的steamID 

```
STEAM_x:x:xxxxxxxx
```

如果网站对中文ID有偏差 可以使用64位ID进行查询

什么？你不知道如何查询64位ID

将steam客户端设置--界面--当可用时显示网站地址栏勾选上

查看个人资料--并复制url栏的64位ID复制到[steamidfinder](https://www.steamidfinder.com/)进行查询

```shell
vim serverfiles/csgo/addons/sourcemod/configs/admins_simple.ini

将你的steamID以

STEAM_x:x:xxxxxxxx "99:z"
```

的格式填入，获取全部权限，重启服务器/重置插件即可拿到!sm/!admin权限（聊天框输入）

## FASTDL部分（文件服务器）

Duck不必, 可以将文件挂载在github并使用cdn加速，省去租用额外服务器与带宽的问题。

许多教程中提到了FASTDL，其实就是一个直链的文件池，以下本教程中提出的方案（现在也是我服务器的方案）

> 来创建一个你服务器专用的仓库吧！
>
> serverfiles/csgo目录下的目录结构有几个主要的资源文件夹
>
> --maps
>
> --materials
>
> --models
>
> --particles
>
> --sound # 注意这个文件夹是没有复数的
>
> 将这些文件夹上传到你新建的仓库，你也可以自己创建这些目录。

> 如何将服务器外部源指向你的文件池

```sh
vim serverfiles/csgo/cfg/csgoserver.cfg
# 访问服务器的全局设置，在此文件中添加
sv_downloadurl "https://cdn.jsdelivr.net/gh/你的用户名/你创建的仓库名"
sv_allowdownload 1
sv_allowupload 1 
# 为了保证你服务器能正确指向到文件池，请将文件层级结构保持与服务器内一致方便服务器为客户端引导文件。
# 此外，jsdelivr并不是唯一可用的cdn，此处只是用例。
```

例如直链一个van darkholme♂的音效

[van_stick](https://cdn.jsdelivr.net/gh/tydaytygx/combatserver/sound/sank/van_stick.mp3)

这样就完成了FASTDL的设置（你需要重启服务器或是重载sourcemod）

> 如何检测你的FASTDL文件池是否正确指向
>
> 使用客户端时打开开发者控制台（~）
>
> 可以查看文件下载进度

# 躲猫猫(Prophunt) 

源仓库 [prophunt](https://github.com/olavim/sm-PropHunt) 

![](https://cdn.jsdelivr.net/gh/tydaytygx/NA/csgo躲猫猫.png)

> 该插件作者已经停止更新，个人外部汉化。
> 针对原版CT方红色遮挡问题，已修改为黑色
>
> > 针对CT蹲下鬼畜的问题，可以将插件设置中的1.5倍速调为正常的1倍速。

# 满十比赛服(Smp)

源帖 [Simple match plugin for CS:GO](https://forums.alliedmods.net/showthread.php?p=2404215)

![](https://cdn.jsdelivr.net/gh/tydaytygx/combatserver/combatserver_smp.png)

> 该插件作者已经停止更新，个人内核汉化，并对源码的编译问题进行了处理

# 多人1V1练枪服务器(Multi1v1)

源仓库 [csgo-multi-1v1](https://github.com/splewis/csgo-multi-1v1)

> 该插件自带多国语言翻译，请移步至源仓库

# 皮肤插件(Playerskin)

源仓库 [playerskin](https://github.com/safra36/PlayerSkin)

![](https://cdn.jsdelivr.net/gh/tydaytygx/NA/CSGO雷姆2.png)

你可以在[gamebanana](https://gamebanana.com/)找到你需要的皮肤/模型，请遵守有关皮肤使用规定。

--教程有待完善

> 该插件自带多国语言翻译，请移步至源仓库

# 语音图标(Speaker Icon (talk icon like in CS:S))

源仓库 [Speaker Icon](https://github.com/Franc1sco/Franug-Speaker-Icon)

![](https://cdn.jsdelivr.net/gh/tydaytygx/NA/csgo_speak_icon.png)

和CS起源一样风格的语音图标

> 前置环境：
>
> [Dhooks](https://forums.alliedmods.net/showpost.php?p=2588686&postcount=589)
>
> [VoiceannounceEX (VoiceHook)](https://forums.alliedmods.net/showthread.php?t=245384)
>
> 该插件即插即用，请移步至源仓库

# 炒鸡投票换图(UMC)

源仓库 [UMC](https://github.com/Silenci0/UMC)

投票换图插件

根据插件需要，你可能还需要一并安装一个maprate插件，不然UMC可能无法正常启动
该插件过于古早 可能不易搜出 仓库中已在
```
UMC/
```
下给出

> 该插件作者已经停止更新，作者对于停止更新表示遗憾并且做了解释。
>
> 个人外部汉化

```sh
#将maprate.smx放入plugins中
#将chi文件夹放入translation中
#运行服务器后修改自动生成/原有的umc_mapcycle.txt即可
vim serverfiles/csgo/umc_mapcycle.txt
"umc_mapcycle"
{
    "Dungeon" //Name this whatever you like
    {

        "maps_invote"   "20"

        "am_anothernuke"
        {
                "weight" "2"
        }


        "am_backrooms"
        {
                "weight" "2"
        }



        "am_crocodile"
        {
                "weight" "2"
        }


        "am_dust2020"
        {
                "weight" "2"
        }


        "am_fabrik_sixteen"
        {
                "weight" "2"
        }


        "am_impound"
        {
                "weight" "2"
        }
以上是使用的例子，一定要确保该地图在服务器的serverfiles/csgo/maps中
不然换图会失败
```

# 舞蹈/跳舞插件( Fortnite Emotes Extended version )

源帖[Fortnite Emotes](https://forums.alliedmods.net/showthread.php?p=2668778)

![](https://cdn.jsdelivr.net/gh/tydaytygx/combatserver/csgomikudance.png)



>将 addons models sound文件夹上传到服务器serverfiles/csgo/目录下
>
>将 models sound 文件夹上传到FASTDL文件池仓库

```sh
# 重要的设置
# 如果你不想让玩家跳舞的时候通过第三人称观测敌人
vim serverfiles/csgo/cfg/sourcemod/fortnite_emotes_extended.cfg
将sm_emotes_hide_enemies "0" 
改为
将sm_emotes_hide_enemies "1" 
# 跳舞时隐藏敌人
```



> 注意：这款插件中包含比较多的模型和音频文件

# 聊天音效插件(Sanky sounds)

源帖[Sankysound](https://forums.alliedmods.net/showthread.php?p=2689260)

聊天框输入文字触发全局音效（例如[van_stick](https://cdn.jsdelivr.net/gh/tydaytygx/combatserver/sound/sank/van_stick.mp3)），多用于娱乐服



> 将config plugins scripting全部上传到服务器serverfiles/csgo/addons/sourcemod/目录下

> 在config/SankSounds.cfg中可以设置触发条件

```sh
"stick" // chat trigger
    {
        "file"  "sank/van_stick.mp3" // path to your file, withouth sound/
    }
```

> 上传音频文件到serverfiles/csgo/addons/sourcemod/sank/目录下
>
> 并在FASTDL文件池仓库sound/sank上传相应的音频文件

[FASTDL部分](#FASTDL部分（文件服务器）)

对于该插件还有一些详细的设置

```sh
vim serverfiles/csgo/cfg/sourcemod/SankSounds.cfg
# 可以删除该文件中sm_sanksounds_accessflag ""
# ""内的flag，重置插件/重启服务器后即可使该插件不受反垃圾系统（AntiSpam system）制约，可以随意使用聊天语音甚至是刷屏
# 下面还有很多关于触发语音的间隔/冷却时间的设置
// This file was auto-generated by SourceMod 
(v1.10.0.6458)
// ConVars for plugin "sanky.smx"


// Access to play sank sounds (limited by AntiSpam system)
// -
// Default: "t"
sm_sanksounds_accessflag ""

// How many sounds can I play in << sm_sanksounds_antispam_time >> time?
// -
// Default: "1"
sm_sanksounds_antispam_soundspertime "1"

// How often I should reset the anti spam timer?
// -
// Default: "90.0"
sm_sanksounds_antispam_time "1.0"

// Access to play sank sounds (no restriction)
// -
// Default: "z"
sm_sanksounds_flagtoavoidantispam "z"

// Time interval to play sounds
// -
// Default: "20.0"
sm_sanksounds_playedsound "3.0"

```

> 你可以在游戏聊天框中输入!sank来决定是否启用聊天音效或者是查看可用的音效触发条件

# 回合回滚(RoundRestore)

>[源帖RoundRestore](**[RoundRestore](https://forums.alliedmods.net/showthread.php?t=325630)** )
>
>用于比赛服的回合重置/回滚插件
>
>~~使管理员拥有时间回溯能力~~
>
>即插即用，个人外部汉化，已向原作者提交了pr
>
>可以前往[源仓库的pull request](https://github.com/Cruze03/CSGO-PugSetup-RoundRestore/pull/2)下载[RestoreRound.phrases.txt](https://github.com/Cruze03/CSGO-PugSetup-RoundRestore/files/5519682/RestoreRound.phrases.txt)

> 食用方法
>
> 安装插件后
>
> ```sh
> sm_restoreround # 控制台
> !sm_restoreround # 聊天框
> 
> ```

# 滚动公告(RoundRestore)

>[源帖](https://forums.alliedmods.net/showthread.php?t=155705)
>
>作者仍在更新，插件本身支持中文 支持多颜色
>
>虽然作者提供了很多的公告提示方法，但是某些广播方式会影响玩家更换武器的操作，没有特别需求建议还是使用 chat 作为广播方法（聊天框滚动提示）

> 食用方法
>
> 安装插件后
>
> ```sh
> vim addons/sourcemod/configs/advertisements.txt
> 
> ```
>
> ```
> "Advertisements"
> {
>     "1"
>     {
>         "chat"        "{green}martinwannamaker"
>     }
>     "2"
>     {
>         "chat"         "新日暮里"
>     }
> }
> ```

# 字体/标签热修复(FixHintColorMessages )

[源帖](https://forums.alliedmods.net/showthread.php?p=2673590#post2673590)

[源仓库](https://github.com/Franc1sco/FixHintColorMessages)

修复插件提示都变成占位符的问题

![](https://cdn.discordapp.com/attachments/335290997317697536/646180265101623338/unknown.png)

# 关于更现代化的比赛服插件
> 一共有两个插件 均支持中文
![](https://cdn.jsdelivr.net/gh/tydaytygx/combatserver/combatserver_smp.png)
## csgo-pug-setup
> [源仓库](https://github.com/splewis/csgo-pug-setup)

## CSGO-PugSetup-RoundRestore 
> [源帖]([源帖](https://forums.alliedmods.net/showthread.php?p=2673590#post2673590))
> [源仓库](https://github.com/Cruze03/CSGO-PugSetup-RoundRestore)

