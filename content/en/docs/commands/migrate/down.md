---
title: "Migrate down"
description: "Migrate down"
lead: ""
draft: false
images: []
menu:
  docs:
    parent: "migrate"
toc: true
---

The `migrate down` command rollbacks selected migration.

After successful operation its `version` is removed from the migration table.
At the beginning of an operation advisory lock is acquired to prevent other operations to run simultaneously.

## Usage

```bash
krab migrate down [set] [version]
```

### Options

- `set` - name of the set to migrate.
- `version` - migration version to rollback.

## Example

For `default` [migration set]({{< ref "docs/configuration/resources/migration-set" >}}) and [migration]({{< ref "docs/configuration/resources/migration" >}}) version `20060102150405` you would use:

```bash
krab migrate down default 20060102150405
```

