---
title: hexo添加文本字数和阅读时间统计
date: 2021-10-10 21:32:21
tags:
- Hexo
categories: Hexo
---

给Hexo添加字数统计和阅读时间统计主要涉及到hexo-wordcount，hexo-symbol-count-time和eslint这三个插件的安装，以及相关配置文件的修改。步骤不复杂，按照步骤操作就可以。

<!--more-->

### 1. 插件安装

实现字数统计和阅读统计涉及到三个插件（hexo-wordcount，hexo-symbols-count-time，eslint），安装命令如下：

```powershell
npm install hexo-wordcount --save
npm install hexo-symbols-count-time --save
npm install eslint --save
```

### 2. 配置文件设置

配置文件涉及到两个，分别是站点配置文件和主题配置文件。站点配置文件为博客根目录下的_config.yml文件，主题配置文件为主题文件夹内的\_config.yml文件。

#### 2.1 站点配置文件

在站点配置文件中添加如下内容：

```yaml
symbols_count_time:
  symbols: true                         # 文章字数统计
  time: true                            # 文章阅读时长
  total_symbols: false                  # 站点总字数统计
  total_time: false                     # 站点总阅读时长
  exclude_codeblock: false              # 排除代码字数统计
```

#### 2.2 主题配置文件

主题配置文件涉及到两处修改，第一处为symbol_count_time，该处原配置文件中已有，只需按照如下内容修改即可：

```yaml
# Post wordcount display settings
# Dependencies: https://github.com/theme-next/hexo-symbols-count-time
symbols_count_time:
  separated_meta: false          # 是否另起一行，true表示不和发表时间一行
  item_text_post: true           # 是否显示文字描述（本文字数，阅读时长）
  item_text_total: false         # 页面底部是否显示文字描述
  awl: 4                         # 每个word的平均长度
  wpm: 275                       # 每分钟阅读的word
  suffix: mins                   # 单位后缀
```

第二处为post_wordcount，此处原配置文件中不包含，可在配置文件最后自行添加如下内容：

```yaml
post_wordcount:
  item_text: true                 # 是否显示文字
  wordcount: true                 # 是否显示字数
  min2read: true                  # 是否显示阅读时间
  totalcount: false               # 是否显示站点总数
  separated_meta: false           # 是否分离
```

### 3. 效果

完成全部设置后，效果如下图所示：

![image-20211010215632724](https://jxliu-picbed.oss-cn-shanghai.aliyuncs.com/img/image-20211010215632724.png)

