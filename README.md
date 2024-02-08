# UTLov使用说明
## 功能介绍
本插件基于OlivOS实现跑团时实时窗口, PL、KP和OB的发言均可以显示在实时窗口中, 并且会将PC、KP和NPC的话实时转语音播放, 掷骰时也会有一点点replay的感觉(有但不多), 使得PL们跑团时更有沉浸感, 希望能鼓励PL们的RP. 此外你也可以使用它来进行跑团直播或是录制当作简单的replay.

实际演示请看视频[https://www.bilibili.com/video/BV144421c7pP/?vd_source=e1ff0185e3ca082e7577b5654e16f3da].

运行截图:
[](https://raw.githubusercontent.com/willkyu/UTLov/main/screenshot.png)

## 使用环境

- 源码运行的OlivOS, 因为我们需要在运行环境里引入其他第三方库, 我这里使用的是3.11版本的python
- 在运行环境中, 需要使用pip安装以下的库:
    - flet == 0.19.0 注意, 这个库只能运行在windows10以上.
    - playsound == 1.2.2 注意, 这个库最好和我的版本一致, 最新版本有bug.
    - baidu-api == 4.16.13
- 此外, 本插件在运行的时候的界面是出现在OlivOS所运行的服务器(电脑)上的, 所以最好该电脑是私人电脑而不是云端服务器.

在满足以上条件后, 我们需要先将`.opk`格式的插件后缀改为`.zip`, 然后解压并打开`UTLovConfig.py`文件. 找到第七行的"Path"配置, 将后面的文件路径改为你的资源文件夹的位置. 

> 如果你不知道资源文件夹是什么, 你只需要随便在一个位置创建一个新的文件夹, 然后输入其地址即可.

保存后重新打包为`.zip`的压缩包, 然后更改后缀为`.opk`, 最后将其放入OlivOS的插件文件夹然后重载插件即可.

## 资源准备

*以下操作都在你的资源文件夹中进行.*

首先是资源文件夹的文件结构如下图所示:

[](https://github.com/willkyu/UTLov/blob/main/files.png)

> 这里我的资源文件夹就是data. 其中, 打码的部分为用户的QQ号.

文件结构为:

- 若干个以QQ号命名的文件夹, 他们是KP或者PL们的资源文件夹.
- 一个名为`bg`的文件夹, 里面放的是若干你需要使用的背景图片. (在本插件中, 图片仅支持`.jpg`或`.png`格式)
- 一个名为`npc`的文件夹, 里面放的是各个npc的资源文件夹.
- 一个名为`ob`的文件夹, 不用管它, 一个空的文件夹即可.
- 一张名为`avatar`的图片, 它是未找到图像时的默认头像, 一般用于不重要的npc.
- 一张名为`dice_bot_avatar`的图片, 它是骰娘的头像.
- 一个名为`dice`的音频文件, 只支持`.mp3`格式, 它是掷骰音效.
- 一张名为`fig`的图片, 它是未找到图像时的默认立绘, 一般用于懒得找立绘的pc.
- 一个名为`UTLov`的配置文件, 后缀是`.ini`, 这是主要的配置文件之一, 可以通过新建文本文档然后改后缀来创建.

下面我们将按顺序来准备这些文件.

### PL的资源文件夹
PL的资源文件夹主要有三个文件:
- 一个名为`avatar`的图片, 它是该PL的PC的头像. 如果没有会自动获取该PL的QQ头像.
- 一个名为`fig`的图片, 它是该PL的PC的立绘. 如果没有会自动使用上述根资源文件夹的名为`fig`的图片.
- 一个名为`userinfo`的配置文件.

#### userinfo.ini

下图是该文件的一个示例.

[](https://github.com/willkyu/UTLov/blob/main/userinfo.png)

其中Main模块中的name和job分别为PC的名字和职业.

audio模块中的spd、pit、vol和per分别为语速(取值0-15，默认为5中语速)、音调(取值0-15，默认为5中语调)、音量(基础音库取值0-9，精品音库取值0-15，默认为5中音量. 取值为0时为音量最小值，并非为无声)和音源(度小宇=1，度小美=0，度逍遥（基础）=3，度丫丫=4，(后面的是精品音库) 度逍遥（精品）=5003，度小鹿=5118，度博文=106，度小童=110，度小萌=111，度米朵=103，度小娇=5)

status中的应该看了都知道什么意思.

下面贴上可以直接复制的示例:
```
[Main]
name = 焦糖色
job = 陪酒妹妹

[audio]
spd = 5
pit = 7
vol = 5
per = 5118

[status]
hp = 11
hpmax = 12
san = 56
sanmax = 60
mp = 12
mpmax = 12
status = 正常
```

### KP的资源文件夹
KP的资源文件夹与PL的类似, 只是没有配置文件`userinfo`.

### 背景图片文件夹
请至少放入一张名为`bg`的图片文件, 其他的背景随意命名.

### npc文件夹
npc文件夹里面包含:
- 若干以NPC名字命名的文件夹, 用于存放各个重要NPC的资源文件, 其与PL文件夹内容类似.
    - 一个名为`avatar`的图片, 它是该NPC的头像.
    - 一个名为`fig`的图片, 它是该NPC的立绘.
    - 一个名为`audio`的配置文件.
- 一张名为`default_avatar`的图片, 它是不重要NPC的头像.
- 一张名为`default_female`的图片, 它是不重要女NPC的立绘.
- 一个名为`default_female`的配置文件, 它的内容与npc文件夹里的配置文件`audio`一致, 它是不重要女NPC的声音配置文件.
- 一张名为`default_male`的图片, 它是不重要男NPC的立绘.
- 一个名为`default_male`的配置文件, 它的内容与npc文件夹里的配置文件`audio`一致, 它是不重要男NPC的声音配置文件.

> 所有没有单独文件夹(以名字命名)的NPC都是不重要NPC

#### audio.ini
下图是该文件的一个示例.

[](https://github.com/willkyu/UTLov/blob/main/audio.png)

同PL的配置文件`userinfo`里的audio模块. 注意, 上述配置文件`default_male`与`default_female`的文件内容与这个一致(具体声音配置可以更改).

### OB文件夹
空文件夹, 运行后如果有人不在配置的PL或是KP中则会被自动归入OB列表, 届时该文件夹会存储他们的QQ头像

### UTLov.ini
主配置文件, 下图是该文件的一个示例.

[](https://github.com/willkyu/UTLov/blob/main/UTLov.png)

其中Main模块的Master_id是用于触发启动命令的QQ号, 一般为运行OlivOS的电脑的所有者. KP_id是KP的QQ号, Group_id是运行该插件的群号. Opening是插件启动时自动用KP配置说出的话, 已被废弃, 可以不要. Bot_name为显示在窗口中的的骰娘名字.

BaiduAPI模块的三个配置请使用自己的Baidu API信息替换, 这是用于百度语音合成的, 具体请百度 **百度语音合成**, 注册会赠送3w次免费试用(好像).

KP_audio模块是KP声音的配置.

下面贴上可以直接复制的示例:

```
[Main]
Master_id = 49xxxxxx8
KP_id = 49xxxxxx8
Group_id = 95xxxxxx5
Opening = 大家好，我是本次测试KP 丘丘。
Bot_name = 黑巧的容器

[BaiduAPI]
APP_ID = 48xxxx84
API_KEY = Ts**************EOe
SECRET_KEY = aG****************************Ld

[KP_audio]
spd = 5
pit = 7
vol = 5
per = 5118
```

## 使用方法
在配置的群聊中使用`.utlov`命令即可开始. 使用`.utlov end`命令即可结束. **注意, 结束后如果要再次使用请重载一次插件**(这是bug懒得修了).

支持的命令:
- rd
- rb
- rp
- ra
- rh
- st hp或san或mp + 运算表达式  (如果掉了1血就`.st hp-1`, 不要直接`.st hp 数值` 这会无法识别从而不能同步进窗口)

特别的命令:
- 以`(`(中英文皆可)开头的PL或KP的发言会进入窗口的meta区(超游区), 并且不会进行语音合成.
- `.sset PC名字 状态` 可以设置窗口中PC的状态, 如果该状态不是"正常", 则其头像会变红. 例如:`.sset 小夜子 疯狂`
- 对于KP, 可以使用`.NPC名字.NPC说的话`来进行NPC发言, 如果是非重要女NPC则需要在第一个点后面加一个英文问号. 例如:
    - 命令`.Mia.大家好,我是Mia`, 如果上述资源准备中有配置npc/Mia的相关资源, 那么会以配置的头像立绘与声音来发言:"大家好,我是Mia".
    - 命令`.路人A.大家好,我是路人A`, 如果上述资源准备中没有配置npc/路人A的相关资源, 那么会以default_male的立绘声音来发言:"大家好,我是路人A"
    - 命令`.?路人B.大家好,我是路人B`, 如果上述资源准备中没有配置npc/路人B的相关资源, 那么会以default_female的立绘声音来发言:"大家好,我是路人B"
- 此外, 不论是KP还是PL, 所有其他以`.`或`。`开头的话不会进行处理(OB除外).


# 写在最后
UTLov这个名字取自我的跑团小群Untitled Tavern的缩写UT加上love, 感谢所有PL们以及和我一起入坑跑团的火龙果.

祝大家次次大成功!
