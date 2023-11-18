---
title: "Environment variables"
description: "Environment variables settings."
lead: ""
draft: false
images: []
toc: true
weight: 2200
---

- `KRAB_DIR` - directory to load configuration from, if not defined, it defaults to the current working directory (default to `/etc/krab` in docker container)
- `DATABASE_URL` - PostgreSQL connection string to use when executing actions
- `KRAB_AUTH` - authentication method, possible values: `none`, `basic` (default: `none`)
- `KRAB_AUTH_BASIC_USERNAME` - username for basic authentication
- `KRAB_AUTH_BASIC_PASSWORD` - password for basic authentication