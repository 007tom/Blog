+++
title = "Homepage"
date = 2020-07-30T15:23:00+08:00
type = "homepage"
draft = false
creator = "Emacs 26.3 (Org mode 9.3.7 + ox-hugo)"
[menu.main]
  weight = 1001
  identifier = "homepage"
+++

**Hi, there👋**

  **Eorg** is a _rich-text_ editor based on [Draft.js](https://draftjs.org/) , inspired by Emacs org-mode
, and it is used to convert `.rtx` to `.tex`

**Try it...**

  打算开发一个富文本编辑器 Eorg （暂时就这样称呼吧），查了一下 Wikipedia，用
`contentEditable` 的比较多，但是 MDN 指出 `.execCommand` 已经过时了，所以就选了 [Draft.js](https://draftjs.org/)
框架实现