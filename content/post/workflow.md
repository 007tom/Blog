+++
title = "WorkFlow"
date = 2020-09-04T12:18:00+08:00
draft = false
creator = "Emacs 27.1 (Org mode 9.3.7 + ox-hugo)"
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

-   `emacs post.org`
-   `C-c, C-e, H`
-   `./deploy.sh "deploy message"`
-   `git add .`
-   `git reset public themes`
-   `git commit -m "some messages"`
-   `git push origin master`


## Eorg {#eorg}

-   update `Eorg` project