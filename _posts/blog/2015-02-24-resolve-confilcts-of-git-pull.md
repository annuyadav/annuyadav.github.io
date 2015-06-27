---
layout: post
title: "Resolve confilcts of git pull and redo a pull again"
author: annu_yadav
modified:
share: true
categories: blog
excerpt:
tags: []
image:
  feature:
date: 2015-02-24T12:10:00+0530
comments: true
---

If your previous pull failed to merge automatically due to conflicts and went to conflict state, 
then you will not be able to do the next git pull untill the conflict is resolved.
   

To solve the problem take the following steps:
   
* Undo the merge and pull again.
    * To undo a merge:
    * git merge --abort
    * git reset --merge

* Resolve the conflict.
* Add and commit the merge.
* Now take a pull again and everything will be fine.
