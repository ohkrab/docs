---
title: "Migration set"
description: "Migration set configuration resource."
lead: ""
draft: false
images: []
menu:
  docs:
    parent: "resources"
toc: true
---

Migration Set is a collection of migrations.

```hcl {lineNos=true}
migration_set "<reference>" {
  schema = "public"

  migrations = [
    ...
  ]
}
```

- `<reference>` - migration set reference name
- `schema` (optional) - schema name where to create `schema_migrations` table and run migrations (`SET search_path TO <schema>` is executed before each migration), default: `public`
- `migrations` - list of [migrations]({{< ref "docs/configuration/resources/migration" >}}) references

## Arguments 

These attributes can be used with arguments:

- `schema` - value is automatically quoted

```hcl {lineNos=true}
migration_set "tenant" {
  arguments {
    arg "schema" {}
  }

  schema = "{{.Args.schema}}"

  migrations = [...]
}
```

## Example

```hcl {lineNos=true}
migration_set "private" {
  migrations = [
    migration.create_tenants
  ]
}

migration "create_tenants" {
  version = "20200628"

  up {
    sql = "CREATE TABLE tenants(name varchar PRIMARY KEY)"
  }

  down {
    sql = "DROP TABLE tenants"
  }
}
```

