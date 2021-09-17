---
title: IntelChina
date: 2021-09-17 16:51:49
tags: ctf
---

留下纪念！~ 5200

{% asset_img image-20210914191217558.png}

2021年年初决定要在这个新的地方开始写博客，结果磨磨蹭蹭到了现在第一篇才出来。恍惚间2021已经只剩下最后三个月了，还是想尽力留下一点什么东西，日后也可有回忆之处。这一年其实发生了很多事情，遇到了很好的人，度过了很愉快的时光，也有挣扎和痛苦。自己的第一篇论文从三月到九月，也终于从一个模糊的想法一步一步落实，一点一点写完，不管最后中不中，好歹是把一件事情从头到尾做完了。

交完论文的第一周，觉得有些无所事事，似乎一下子心就空了，好在友人相邀，打一场入门级的CTF。为期一天的比赛中，我逐渐把之前卸载掉的工具又下载回来，比赛过程此时回想起来竟是感慨无比，那些熟悉的步骤，我以为自己已经忘了，事实上随手可以拾起。

虽然最后没能AK，但好像这几个数字还不错。

我与我周旋久，宁作我。

### Reverse

#### Among Them

签到题的水平，不知道为什么给两星的难度。直接丢进IDA，main函数F5。

{% asset_img image-20210914171210184.png}

这鬼东西双击进去就看到了flag

{% asset_img image-20210914171246670.png}

flag为：HTB{4r3_u_r34lly_th3_us3r_!@#$%^&*()??}

值得注意一下的是，这道题用ptrace增加了反调试，虽然我不知道一道不用调试的题目为什么要这么严谨的加反调试，但是讲一下这里要怎么绕过反调试。

{% asset_img image-20210914171519107.png}

直接把exit(1)的汇编代码部分nop掉。

{% asset_img image-20210914171732329.png}

{% asset_img image-20210914171807071.png}

{% asset_img image-20210914171831706.png}

把Intruction部分删掉，填nop

{% asset_img image-20210914171930882.png}

你再把main函数F5回去，就可以看到没有这个退出的指令了，可以动态调试了。

{% asset_img image-20210914172049341.png}

#### Curse

这道题是python打包的可执行文件的逆向。然后hint也说了是python3.8。

{% asset_img image-20210914173053736.png}

首先python要安装PyInstaller。

直接

```
pip install pyinstaller
```

安装后在python的安装目录下找到archive_viewer.py这个文件，比如我的路径是

```
home/miniconda3/lib/python3.8/site-packages/PyInstaller/utils/cliutils
```

将文件curse也放在这个目录下，然后运行archive_viewer.py

{% asset_img image-20210914184734360.png}

{% asset_img image-20210914185102710.png}

然后看看pyc文件的文件头有没有缺失，如果有缺失，要补上。

{% asset_img image-20210914185303907.png}

{% asset_img image-20210914185315968.png}

果然缺了16个字节，给补上。

{% asset_img image-20210914185354550.png}

{% asset_img image-20210914185420606.png}

随便找个在线反编译工具，给curse.pyc文件反编译成.py文件。

{% asset_img image-20210914185600630.png}

解出password就好了。

flag为HTB{pyth0n_r3v_1s_ez}

### Forensics

#### clang

是一个.pcap文件，用wireshark打开，是SIP协议和RTP协议，RTP协议是会话通信协议。

https://blog.csdn.net/weixin_39934520/article/details/108014027

**在“电话 - VoIP电话”菜单之后，您可以看到捕获文件中的哪些连接并收听。某些VoIP编解码器实现了此功能。**

{% asset_img image-20210914165404085.png}

播放后听到：3242459345，flag为 HTB{3242459345}

PS：左声道听

#### Instructions

docm文件是docx带宏的文件，直接用word打开，搜索HTB。

{% asset_img image-20210914170402555.png}

```
str = "https://windowsliveupdater.com/HTB{" + Chr(110) + "3" + Chr(Asc("w")) + "_VP" + Chr(78) + "_" + Chr(Asc("n")) + "3" + Chr(119) + "_b4ck"`
 sec = Replace("door}/bin", "o", "0")
```

flag为：HTB{n3w_VPN_n3w_b4ckd00r}

### Web

#### Potent Quotes

SQL注入。单引号的，注意要加注释。

{% asset_img image-20210916154917033.png}

#### The order

xml：https://blog.csdn.net/m0_46580995/article/details/107450111

flag是：HTB{who_let_the_xxe_out??}

#### Rescue Mission

注册一个账号登录进去，生成nginx配置。仔细查看配置信息，发现/storage/ 目录配置了autoindex on，也就说明可以浏览目录进行下载

直接访问/storage/ 目录，发现有个backup文件，果断下载下来。解压之后发现是一个sqlite文件。然后我下了Navicat这个软件来看。

{% asset_img image-20210916153754580.png}

看users表，第一个用户就是admin，然后password是MD5，给他解密下。直接用的在线的工具，这年头好多网站要解密MD5居然要收费，我用的这个：https://www.somd5.com/

{% asset_img image-20210916153905815.png}

{% asset_img image-20210916154009425.png}

然后就直接了，登录进admin，就看到flag了

{% asset_img image-20210916154156483.png}

flag是：HTB{nginx_is_really_spilling_the_beans_here...}

#### SimPlay

任意命令执行。先ls \一下看看有没有跟flag有关的文件，然后打开这个文件就行了。

{% asset_img image-20210916155235416.png}

flag是：HTB{3v4l_h4s_put_y0ur_3v1l_pl4ns_und3r!}

### Crypto

#### Lost



#### Secret Package



#### Simple RSA



#### Sleeper Agents





