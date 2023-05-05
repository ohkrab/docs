---
title: "Release v0.8.0 notes"
description: "Transaction attribute for actions."
summary: "Ability to run actions without transaction."
date: 2023-05-05T18:32:00+0100
draft: false
images: []
categories: ["News"]
tags: ["release", "krab", "actions"]
contributors: ["qbart"]
pinned: false
homepage: false
---

Newest release brings small addition to how actions are run.
Consider following scenario when you want to create database and assign owner.
Normally action is defined as follows:

```hcl
action "db" "create" {
  arguments {
    arg "name" {
      description = "Database name"
    }

    arg "user" {
      description = "Database user"
    }
  }

  sql = "CREATE DATABASE {{ .Args.name  }} OWNER {{ .Args.user }}"
}
```

Run action:

```
krab action db create -name animals -user api
```

This will fail with `CREATE DATABASE cannot run inside a transaction block SQL state: 25001`.

[Actions]({{< ref "docs/resources/action" >}}) can accept transaction attribute that determines how action is run. New action would take the following form:
```hcl
action "db" "create" {
  arguments {
    arg "name" {
      description = "Database name"
    }

    arg "user" {
      description = "Database user"
    }
  }

  transaction = false
  sql         = "CREATE DATABASE {{ .Args.name  }} OWNER {{ .Args.user }}"
}
```

`transaction = false` will disable transaction (similiary how [migrations]({{< ref "docs/resources/migration" >}}) can be run) and action should run without an error.
