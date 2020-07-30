+++
title = "WorkFlow"
date = 2020-07-30T19:37:00+08:00
draft = false
creator = "Emacs 26.3 (Org mode 9.3.7 + ox-hugo)"
[menu.implementation]
  weight = 1003
  identifier = "workflow"
+++

My folder structure:

```bash
.
├── Eorg
└── blog
    ├── deploy.sh
    └── docs
      └── post.org
```


## blog {#blog}

-   edit `post.org`
-   `./deploy.sh "deploy message"`
-   `git add .`
-   `git reset public`
-   `git reset themes`


## Eorg {#eorg}

-   update `Eorg` project