---
title: Hexo
date: 2025.06.21 00:40
updated: 2025.06.21 16:40
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
summary: 为什么搭建个人博客呢？去年看到杜老的知识库时，就有搭建个人博客的想法。后一次旁听了杜老给研究生开的组会，组会上也听杜老让研究生自己搭建博客记录，遂有此站来记录一些自己的想法和经验（现在才大二，经验不多，慢慢累积总会有嘛），分享一下自己感觉使用有用的技巧，说不定几年后回头看觉得当时遇到的问题可能有更优解。                                                       # stay blank if you don't want to show summary. Markdown available. [如果不想显示摘要请留空。支持Markdown格式。]
notes_before:                                                   # Anything you want to show at the beginning. Markdown available. [你想在开头显示的任何内容。支持Markdown格式。]
notes_after:                                                    # Anything you want to show at the end. Markdown available. [你想在结尾显示的任何内容。支持Markdown格式。]
---
个人博客搭建
本博客的搭建参考[爱扑bug的熊](https://www.bilibili.com/video/BV1qD4y1z783/?spm_id_from=888.80997.embed_other.whitelist&t=15.817036&bvid=BV1qD4y1z783&vd_source=210e1aa610dbebb024dea3b9061c257d)

搭建方案为：hexo + github + netlify + cloudflare
- 使用hexo框架，博客文件采用GitHub托管，netlify部署博客网站，cloudflare加速
- 该方案不需要购买服务器也不需要备案，只需要购买域名，使用cloudflare加速即可
- 原教程博主使用的是Mac系统，我是Windows系统，但是实际操作并未太大差别

环境配置
安装node.js，参考网上教程下载安装，然后在系统环境变量中添加node.js路径，确保可以全局使用
我之前做前端Vue项目时，node.js是全局安装过，所以这里没有重新安装

检查node.js和npm是否安装成功
```bash
node -v   # 查看node.js版本
npm -v   # 查看npm版本
```

npm默认官网源下载较慢，可以切换为淘宝源
```bash
npm config set registry https://registry.npmmirror.com
```

博客搭建
安装
有了npm包管理工具可以直接指令安装hexo
```bash
npm install -g hexo-cli    # 全局安装hexo
```
-g 表示全局安装，安装后可以在任何地方使用hexo命令

这里有一个问题我先按下不表，后文会提

初始化博客
执行命令
```bash
hexo init 博客项目名称    # 初始化博客
```

等待片刻后，终端显示
```bash
INFO  Cloning hexo-starter https://github.com/hexojs/hexo-starter.git
INFO  Install dependencies
# 一些可能的中间信息
INFO  Start blogging with Hexo!
```

出现Start blogging with Hexo!表示初始化完成

进入博客目录
```bash
cd 博客项目名称
```

安装所需依赖
```bash
npm install  # 安装的依赖项在package.json文件的dependencies字段中可以看到
```

启动博客
```bash
hexo server    # 本地运行server服务预览，浏览器打开http://localhost:4000即可看到博客
```

添加建站脚本
为了后续netlify部署方便，我们在ackage.json里面添加一个命令：
```json
{
    
    "scripts": {
        "build": "hexo generate",
        "clean": "hexo clean",
        "deploy": "hexo deploy",
        "server": "hexo server",
        "netlify": "npm run clean && npm run build" // 这一行为新加
    },
    
}
```

