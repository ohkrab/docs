---
title: "File system"
description: "Functions releated to file system."
lead: "Functions that can be used in krab configuration files."
draft: false
images: []
menu:
  docs:
    parent: "functions"
weight: 10
toc: true
---

## file_read

 `file_read` reads a file at the given path and returns its content as a string.

```hcl
sql = file_read("path")
```

