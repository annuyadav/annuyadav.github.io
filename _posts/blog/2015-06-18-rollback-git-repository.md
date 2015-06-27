---
layout: post
title: "Rolling back git repository by one commit"
author: annu_yadav
modified:
share: true
categories: blog
excerpt:
tags: []
image:
  feature:
date: 2015-06-18T15:28:00+0530
comments: true
---

Set the local branch one revision back by:

```
    git reset --hard HEAD^
```

HEAD^ means one revision back.
After this push the changes to origin:

```
     git push -f
```

-f is for force push because if force pushing is not done then git would recognize that our branch is behind origin by one commit and nothing will be done.

By force pushing we tell git to overwrite HEAD in remote repository. Force push will delete previous commit and will keep new commit.
