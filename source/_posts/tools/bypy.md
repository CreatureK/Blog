---
title: Bypy 百度网盘
date: 2025.06.19 21:56
updated: 2025.06.21 16:04
categories:
- 实用工具
tags:
- 工具
- 技术分享

password:                                                       #加密
layout: post
type: fiction
toc: true                                                       # true: show toc | false: disable toc [true: 显示目录 | false: 禁用目录]
meta_show: false                                                 # true: show meta info | false: disable meta info [true: 显示元信息 | false: 禁用元信息]
meta_info:
  type:                                                         # Original Work | Fanfiction | ... [原创作品 | 同人作品 | ...]
  fandom:                                                       # If type is 'Fanction', it's generally necessary to indicate the fandom | stay blank if you don't want to show fandom info [如果类型是'同人作品'，通常需要指明粉丝群体 | 如果不想显示粉丝群体信息请留空]
  relationship:                                                 # stay blank if you don't want to show relationships info [如果不想显示关系信息请留空]
  character:                                                    # stay blank if you don't want to show characters info [如果不想显示角色信息请留空]
  rating:                                                       # General | Teen | Mature | Explicit | stay blank if you don't want to show rating [普通 | 青少年 | 成熟 | 限制级 | 如果不想显示分级请留空]
  warning:                                                      # Anything that audiences should know before reading | stay blank if you don't want to show warnings [读者在阅读前应该知道的任何事项 | 如果不想显示警告请留空]
status:                                                         # Y | N | stay blank if you don't want to show status [是 | 否 | 如果不想显示状态请留空]
summary: 今天帮兵哥将llm项目组的数据从百度网盘保存到本地，经过搜索发现的一个实用工具bypy                                                       # stay blank if you don't want to show summary. Markdown available. [如果不想显示摘要请留空。支持Markdown格式。]
notes_before:                                                   # Anything you want to show at the beginning. Markdown available. [你想在开头显示的任何内容。支持Markdown格式。]
notes_after:                                                    # Anything you want to show at the end. Markdown available. [你想在结尾显示的任何内容。支持Markdown格式。]
---

# Bypy 百度网盘

**Bypy** 是一个百度云/百度网盘的 Python 客户端。主要的目的就是在 Linux 环境下（Windows 下也可用）通过命令行来使用百度云盘。

由于百度 PCS API 权限限制，程序只能存取百度云端 `/apps/bypy` 目录下面的文件和目录。

- 应用方面对于自己的 PC 而言其实效果和直接使用百度网盘效果一样
- 但是对于远程服务器而言，则可以使用 bypy 进行长时间传输和下载，直接将服务器和百度网盘相连
- 但是注意：bypy 本身传输速度依旧是账号等级对应的速度

## Aria2 加速下载

搜索到可以通过 aria2 第三方下载器加速下载（我没使用过，之后可以尝试）

### 安装 aria2

```bash
conda create -n aria2
conda activate aria2
conda install -c conda-forge aria2
```

### Aria2 加速

使用参数 `--downloader aria2` 让 bypy 调用 aria2 下载
`--downloader-arguments` 设置 aria2 的参数，默认为 `-c -k10M -x4 -s4 --file-allocation=none`

```bash
bypy --downloader aria2 download <remotefile> [localpath]
bypy --downloader aria2 download [remotedir] [localdir]

# 修改参数使速度最大化
bypy --downloader aria2 --downloader-arguments='-c -k10M -x16 -s16 --file-allocation=none' download <remotefile> [localpath]
```

## Bypy 使用

### 安装

安装需要有 Python 环境，安装 pip 后进行 bypy 的下载

```bash
pip install bypy
```

### 登录

安装成功之后命令行执行，进行登陆操作

该指令执行后会给出链接，ctrl+click 打开后会跳转到百度网盘的登录页面，登录成功后会返回一串字符，复制粘贴到命令行回车即可

```bash
bypy info
```

### 基本操作

想要详细了解 bypy 的某一个指令：

```bash
bypy help <command>
```

显示云盘列表：

```bash
bypy list
```

### 常用操作

```bash
# 下载
bypy download <remotefile> [localpath]
bypy syncdown

# 上传
bypy upload <localpath> <remotedir>
bypy syncup

# 比较本地目录和云盘目录
bypy compare
```

### 调试信息

- 运行时添加 `-v` 参数，会显示进度详情。
- 运行时添加 `-d`，会显示一些调试信息。
- 运行时添加 `-ddd`，还会会显示 HTTP 通讯信息（警告：非常多）

## 参考资料

[Bypy仓库](https://github.com/houtianze/bypy "Bypy仓库链接")

## 个人使用时遇到的问题

百度网盘下不存在 apps (即我的应用数据)该文件夹，一开始我在找该文件夹浪费了一些时间，尝试自己创建名为"我的应用数据"文件夹，很明显不对。

上网搜索解决，2019 年的一个帖子说，直接创建文件夹命名为 apps 即可，实际操作发现现在已经 2025 年了，该操作会导致百度网盘提示：apps 为关键字，无法创建。

最后想着不如直接上手 bypy 试试情况，在执行 `bypy list` 指令后，百度网盘自己会创建出 `apps/bypy` 文件夹，只用把需要上传到服务器的数据全部放在 `apps/bypy` 文件夹下，就可以直接使用 bypy 进行上传下载了。
