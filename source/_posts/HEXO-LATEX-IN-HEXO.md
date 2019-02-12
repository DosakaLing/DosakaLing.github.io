---
title: '[ HEXO ] LATEX IN HEXO'
date: 2019-02-12 15:51:13
tags:
	- Hexo
	- Writing
categories:
	- Hexo
	- Setting
mathjax: true
---

# [ HEXO ] Writing Math Formulas In HEXO

- Reference: [ShallowLearner's Blog](https://www.jianshu.com/p/7ab21c7f0674)
- The notes covers:**change the renderer in hexo**, **open the support of LATEX in Hexo**

<!--more-->

# Change the engine

```bash
npm uninstall hexo-renderer-marked --save
npm install hexo-renderer-kramed --save
```

# Modify the escape string

- open **node_modules\kramed\lib\rules\inline.js**

```js
//  escape: /^\\([\\`*{}\[\]()#$+\-.!_>])/,
escape: /^\\([`*\[\]()#$+\-.!_>])/,
//  em: /^\b_((?:__|[\s\S])+?)_\b|^\*((?:\*\*|[\s\S])+?)\*(?!\*)/,
em: /^\*((?:\*\*|[\s\S])+?)\*(?!\*)/
```

- restart hexo, use command **hexo clean** and **hexo generate**

# Configure the theme

- open **themes/next/_config.yml**
- look for math and set **enable:** to True