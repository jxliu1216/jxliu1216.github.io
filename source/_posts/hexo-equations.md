---
title: hexo博客公式渲染
date: 2021-10-25 23:13:14
tags:
- Hexo
categories: Hexo
---

NexT提供了两个渲染引擎（MathJax和Katex）用来显示公式，本文介绍Hexo NexT主题下配置MathJax进行数学公式渲染。

<!--more-->

### 1. 安装插件

如果使用MathJax进行公式渲染，需要安装如下两个插件之一：hexo-renderer-pandoc或者hexo-renderer-kramed（不推荐），本文采用hexo-renderer-pandoc，运行如下命令：

```bash
npm uninstall hexo-renderer-marked --save         # 卸载原有的渲染插件
npm install hexo-renderer-pandoc --save           # 安装hexo-renderer-pandoc插件
```

hexo-renderer-pandoc正常运行还需要安装pandoc软件，软件下载链接如下：[pandoc](https://github.com/jgm/pandoc/releases)。安装完成后，重启电脑，使得相关配置生效。

### 2. 配置

对next/_config.yml中的math部分进行如下配置：

```yaml
math:
  ...
  per_page: true
  ...
  mathjax:
    enable: true
```

per_page参数的含义是：是否对每一篇博客都按需进行数学公式渲染。如果设置为false，则默认对每篇博客都进行数学公式渲染。若设置为true，则按需对博客进行数学公式渲染。对于需要开启数学公式渲染的博客，在其Markdown文件的Front-matter部分添加mathjax: true，如下所示：

```markdown
<!-- This post will render the Math Equations -->
---
title: 'Will Render Math'
mathjax: true
---
....
```

上述两步执行完毕后，执行如下命令，即可正常渲染公式：

```bash
hexo clean
hexo g
hexo s
```

### 3. 注意事项

1. 行内数学公式显示（\$...\$）：在第一个\$后和第二个\$前，不要有空格，否则无法正常渲染公式。
