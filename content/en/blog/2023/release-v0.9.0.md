---
title: "Release v0.9.0 notes"
description: "UI for managing actions with some database metadata."
summary: "UI for managing actions"
date: 2023-11-18T13:19:00+0100
draft: false
images: []
categories: ["News"]
tags: ["release", "krab", "actions", "ui", "auth"]
contributors: ["qbart"]
pinned: false
homepage: false
---

Newest release adds UI with simple authentication. UI allows to exececute actions and browse (in a very limited form for now) database metadata.

Consider following action:

```hcl
action "db" "create" {
  description = "Create database"

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

Action description is a new attribute and is required.

Normally action would be run with:

```
krab action db create -name animals -user api
```

But now you can deploy `krab` as a service. It will expose UI on `http://localhost:8888` (you can use ready to use docker image).

In order to enable http basic authentication you need to set environment variables:
```
KRAB_AUTH=basic
KRAB_AUTH_BASIC_USERNAME=krab
KRAB_AUTH_BASIC_PASSWORD=secret
```

Action list:
![Actions - List](/images/2023/dbaction.png)

Execute action:
![Actions - Run](/images/2023/dbrun.png)

Database list:
![Database - List](/images/2023/dblist.png)