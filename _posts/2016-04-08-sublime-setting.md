---
layout: post
title: 我的极简 Markdown 编辑器
date: 2016-04-08
tags: [读书,卡片]
description: 世上无难事，只怕笨方法。
---



用过 

- Macdown
- Mou
- Mweb
- Ulysses
- Textmate

还是觉得 Sublime Text 搭配 Markdown 的书写体验最佳。

分享我的Sublime Text 插件给大家：

### 安装插件方法

- 安装 Package Control
	-[Installation - Package Control](https://packagecontrol.io/installation)
- Package 安装 的方法
	+ ⌘+⇧+P 进入 Package Control: Install Package 进行搜索安装
	+ 手动下载好 Package 放置进 sublime Packages 目录
		* Sublime -> Preference -> Browse Packages

### 插件列表 

- MarkdownEditing
	+ 可使用 Markdown 快捷键
		* ⌘+⌥+V 插入链接
		* ⌘+⇧+K 插入图片链接
		* ⌘+⌥+B 加粗
		* ⌘+^+1…6 快速插入标题
		* ⌥+⇧+6 快速插入文档脚注（footnotes）
- Markdown Extended
	+ 如在打开 md 文件时显示的颜色主题是 Markdown Light，你可以点击右下角来切换主题格式，切换默认主题可在格式列表的最顶，点击 「open all with current extensions as ……」点击你想要的格式即可。[在 Sublime 中配置 Markdown 环境 - Blog of 太极儒](http://frank19900731.github.io/blog/2015/04/13/zai-sublime-zhong-pei-zhi-markdown-huan-jing/)
- Default File Type
	+ 可设置默认新建文件是 md
- Clickable URLs
	+ 可显示页面中的链接
- 预览插件 Markdown Preview
	+ 手动在 Preferences -> Key Bindings User 配置预览快捷键
	+ { "keys": ["alt+p"], "command": "markdown_preview", "args": { "target": "browser"} }
	+ [sublime text 2 下的Markdown写作 - 简书](http://www.jianshu.com/p/378338f10263)
- 颜色主题
	+ Material Theme 个人暂时最爱，无序列表有标记
	+ Theme - itg.flat 小清新，无序列表无标记
- SideBarFolders
- SyncedSidebarBg
	+ 可以将侧栏的背景颜色自动调节成与主题一致
- [WordCount - Packages - Package Control](https://packagecontrol.io/packages/WordCount)
	+ 可统计选定的字数

### 参考资料

- [如何优雅地使用Sublime Text | 晚晴幽草轩](http://www.jeffjade.com/2015/12/15/2015-04-17-toss-sublime-text/)
- [近乎完美的 Markdown 写作体验 - Sublime Text 3 + OmniMarkupPreviewer](http://macplay.leanote.com/post/%E8%BF%91%E4%B9%8E%E5%AE%8C%E7%BE%8E%E7%9A%84-Markdown-%E5%86%99%E4%BD%9C%E4%BD%93%E9%AA%8C-Sublime-Text-3-OmniMarkupPreviewer)

祝开心

陈素封 谨上

----

**六经注我**

更多[好物清单](https://github.com/cnfeat/GoodThingList)，哈哈哈。


