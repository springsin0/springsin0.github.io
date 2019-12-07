---
layout: post
title:  Wolf RPG游戏汉化工具Wolf Trans
date:   2019-12-01 12:00:00 +0800
categories: 汉化
tags: [工具, 翻译, 游戏]
---
(English description is [*Below*](#wolf-trans), please pull down)

Wolf Trans是一个Wolf RPG游戏的汉化工具/辅助翻译工具，用于减少汉化/本地化Wolf RPG游戏的难度和工作量。它将游戏数据文件(.project, .dat, .mps)中所有可翻译的文字内容抽出为txt文本文件，翻译者翻译编辑txt文件后，它再将翻译后的文字整合进数据文件重新输出(.project, .dat, .mps)。
此版本以原作者Eliza的开源项目<https://github.com/elizagamedev/wolftrans> 为基础略作修改，支持中文汉字，原版只能使用日文汉字。
<!-- more -->

WolfTrans抽出的txt是由一个个类似这样的翻译段组成的：

    > BEGIN STRING
    【はじめから】
    > CONTEXT MPS:Map000/events/0/pages/1/36/Picture < UNTRANSLATED
    
    > END STRING

\> BEGIN STRING和> CONTEXT之间的日语原文，不要动。翻译者只需要将译文填入最后一个> CONTEXT 和> END STRING之间即可

    > BEGIN STRING
    【はじめから】
    > CONTEXT MPS:Map000/events/0/pages/1/36/Picture < UNTRANSLATED
    【从头开始】
    > END STRING

* 项目及下载：<https://github.com/springsin0/wolftrans/>
* 关联文章：[Wolf RPG游戏汉化翻译全流程说明]({% post_url 2019-12-01-Wolf RPG游戏汉化翻译全流程说明 %})

## 安装
下载所有源代码后解压到一个文件夹即可

另外本工具使用ruby编写，你需要安装ruby，这里下载http://rubyinstaller.org/downloads/

## 使用  
1.运行wolftrans，从游戏数据文件中抽出可翻译内容生成txt文本文件  
2.打开txt文件，翻译、编辑、保存  
3.再次运行wolftrans（命令参数与第1步完全相同），根据txt文本文件中的翻译后内容重新生成游戏数据文件

确认你已经安装好了ruby，需要使用ruby运行Wolf Trans安装的文件夹下**bin**文件夹里的**wolftrans**，例如:
    
    ruby d:\wolftrans\bin\wolftrans 
    
wolftrans是一个控制台应用程序，以命令行的方式运行。用法和参数如下:

    wolftrans game_dir patch_dir out_dir [localecode]

`game_dir`是你要翻译的原游戏文件夹，要求游戏数据文件是非加密和不打包的，即是.project, .dat, .mps文件而不是.wolf文件。

`patch_dir`用于存放可翻译文本文件，如果`patch_dir`不存在，它将自动建立并从`game_dir`中抽出的可翻译文本生成txt文件存于此文件夹。

`out_dir`将用于存放整合了翻译文本后重新输出的游戏数据文件。

**警告：软件每次运行都会先清空`out_dir`的所有内容，请小心**

`localecode`: 此参数用于设置翻译语言的编码代码页，默认（不设置此参数）为英文。例如GBK为简体中文, CP950为繁体中文, CP949为韩文...更多请参考https://docs.ruby-lang.org/ja/latest/class/Encoding.html 示例：

当目标翻译语言为英文时：

    ruby d:\wolftrans\bin\wolftrans "D:\gamename" "D:\transwork\patch" "D:\transwork\output"
    
当目标翻译语言为简体中文时：

    ruby d:\wolftrans\bin\wolftrans "D:\gamename" "D:\transwork\patch" "D:\transwork\output" "GBK"

## 提示
一个典型的`patch_dir`文件夹结构如下：

    ─patch
        ├─Assets
        └─Patch
            └─dump
                ├─common
                ├─db
                │  ├─CDataBase
                │  └─DataBase
                └─mps
                
需要翻译的是里面的txt文件

本软件会将相同的文字内容抽出为同一个翻译段，并将它放在首次出现的地方对应的txt文件中。比如游戏数据中多个地方都有“はい”这句话，抽出的翻译段如下，存于`patch_dir`\Patch\dump\mps\Map000.txt 中

    > BEGIN STRING
    はい
    > CONTEXT MPS:Map001/events/0/pages/1/23/Choices
    > CONTEXT MPS:Map002_A_1/events/0/pages/1/23/Choices
    > CONTEXT MPS:Map002_A_3/events/4/pages/7/3/Choices
    > CONTEXT MPS:Map002_A_3/events/9/pages/2/21/Choices
    > CONTEXT MPS:Map003/events/5/pages/1/3/Choices
    > CONTEXT MPS:Map003/events/24/pages/2/4/Choices
    > CONTEXT MPS:Map003/events/32/pages/2/4/Choices
    > CONTEXT MPS:Map003/events/34/pages/2/5/Choices
    > CONTEXT MPS:Map004/events/5/pages/2/4/Choices
    > CONTEXT MPS:Map004/events/9/pages/2/12/Choices
    > CONTEXT MPS:Map004/events/20/pages/2/6/Choices
    是
    > END STRING

这样只需要在map000.txt中翻译一次，其他的地方也都会自动生成翻译后的数据文件(.project, .dat, .mps)。但这也带来一个问题，“はい”在不同的语境上下文中可能有不同的意思，比如表示“知道了”、“好”等等，统一翻译成“是”比较生硬。此时可以自己在相应的地方写一个翻译段，比如在map004.txt里，

    > BEGIN STRING
    はい
    > CONTEXT MPS:Map004/events/5/pages/2/4/Choices
    好
    > END STRING
    
就能实现同一原文不同译文。

另外，本软件生成的可翻译txt的内容不一定是必须要翻译的。ADV/AVG游戏一般不需要翻译RPG游戏特有的升级、战斗等相关内容，如这些文件：

    "D:\transwork\patch\Patch\dump\db\DataBase\戦闘コマンド.txt"

## 声明
作者对使用此软件及产生的后果概不负责。为了避免意外，防止翻译成果付诸东流，建议请经常备份`patch_dir`里的内容。

----------------------------------------------------------------------------------------------------
# Wolf Trans
A translation tool for Wolf RPG Editor games
![](http://i.imgur.com/fzuJjsU.png)

## Summary
Wolf Trans is a set of tools to aid in the translation of games made using
[Wolf RPG Editor](http://www.silversecond.com/WolfRPGEditor/). The syntax and functionality is inspired primarily by the [RPG Maker Trans](http://rpgmakertrans.bitbucket.org/) project.  
This version is based on original author Eliza's project <https://github.com/elizagamedev/wolftrans>, made some small changes, support Chinese charaters, original version can only use Japanese charaters.

## Installation
Download all source code zip file and uncompress to a folder.
If you are using Windows, you will need to have Ruby installed first. You can download it from [here](http://rubyinstaller.org/downloads/).

## Example
The source code for the translation patch file used in the sample image above is the following:

    > BEGIN STRING
    ウルファール
    「ようこそ、\\r[WOLF,ウルフ] RPGエディターの世界へ！
    　私は案内人のウルファールと申します。
    > CONTEXT MPS:TitleMap/events/0/pages/1/65/Message
    Wolfarl
    "Welcome to the world of WOLF RPG Editor!
     I am Wolfarl, your guide."
    > END STRING

All of the translatable game text is first extracted by Wolf Trans into plaintext files, which can then be edited by a translator. Refer to RPG Maker Trans's documentation for now for a more thorough explanation, as the two share very similar designs.

## Usage  
1.Run wolftrans, extract translatable content from game data files and generate txt text files.  
2.Open txt files, translate, edit and save.  
3.Run wolftrans again(command parameters are same as step 1), rebuild game data files according to translated content in txt files.

Make sure you have already installed ruby. You need to run **wolftrans** in the **bin** folder of the Wolf Trans installed folder by using **ruby**. For exsample:
    
    ruby d:\wolftrans\bin\wolftrans 
    
Currently, Wolf Trans can be invoked with the command line:

    wolftrans game_dir patch_dir out_dir [localecode]

`game_dir` is the original game's folder you want to translate. The game data files should be unencrypted and unpackaged, namely .project, .dat, .mps file but not .wolf file.

`patch_dir` is used to save translatable text file. If `patch_dir` does not exist, it will be automatically generated with the text contained in the data files of the game in `game_dir`. 

`out_dir` will contain the patched game.

**WARNING: OUT_DIR WILL BE *DELETED* IF IT EXISTS. USE CAUTION.**

`localecode`: Set Encoding CodePage for other languages, default(not set) for English. eg. GBK for Simplified Chinese, CP950 for Traditional Chinese, CP949 for Korean... About Encoding CodePage, See this <https://docs.ruby-lang.org/ja/latest/class/Encoding.html> e.g.

When destination translated language is English:

    ruby d:\wolftrans\bin\wolftrans "D:\gamename" "D:\transwork\patch" "D:\transwork\output"
    
When destination translated language is Simplified Chinese:

    ruby d:\wolftrans\bin\wolftrans "D:\gamename" "D:\transwork\patch" "D:\transwork\output" "GBK"

## Disclaimer

This project is still in a very early state, and probably won't work with all games. Make sure to backup your translation work frequently in case of errors until this project is stable. I am not personally responsible for anything done by this tool.
