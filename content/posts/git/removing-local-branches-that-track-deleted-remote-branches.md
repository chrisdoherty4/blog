---
title: "Removing Local Branches That Track Deleted Remote Branches"
date: 2020-10-09T14:52:05-06:00

tags:
    - 

categories:
    - 

draft: true
---

```
git branch -vv | grep ": gone]" | awk '{print $1}' | grep -v "*" | xargs git branch -D
```

