---
title: "Quick start"
description: "Quick setup for krab example."
lead: "Quick example of how to use krab."
images: []
menu:
  docs:
    parent: "get-started"
weight: 1300
toc: true
---

## Generate migration

Go to your project (`/tmp` folder is used as an example):

```shell
mkdir -p /tmp/project
cd /tmp/project
```

and generate migration:

```
krab gen migration -name create_animals id name:varchar color:varchar weight:int timestamps
```

This will generate file and print the output:

```none
File generated:
/tmp/project/db/migrations/20230213_210359_create_animals.krab.hcl
Don't forget to add your migration to migration_set:

    migration_set "public" {
      migrations = [
        ...
        migration.create_animals,
        ...
      ]
    }
```

File will contain:

```hcl
migration "create_animals" {
  version = "20230213_210359"

  up {
    create_table "animals" {
      column "id" "bigint" {
        identity {}
      }
      column "name" "varchar" {}
      column "color" "varchar" {}
      column "weight" "int" {}
      column "created_at" "timestamptz" {
        null = false
      }
      column "updated_at" "timestamptz" {
        null = false
      }
      primary_key {
        columns = ["id"]
      }
    }
  }

  down {
    drop_table "animals" {}
  }
}
```

Since this is your first [migration]({{< ref "docs/resources/migration" >}}) you need to create [migration set]({{< ref "docs/resources/migration-set" >}}) that holds
the list of migrations. Migration sets allowed to separate migrations into different contexts/boundaries and apply them separately.
Let's create one:

```shell
touch db/migration_sets.krab.hcl
```

And paste the following content:

```hcl
migration_set "public" {
  migrations = [
    migration.create_animals,
  ]
}
```

{{<alert context="info">}}
Krab traverses your whole directory recursively and looks for `*.krab.hcl` files
so file paths don't really matter, `db/migrations` is just a generator default.
Structure is up to you.
{{</alert>}}

{{<alert context="warning">}}
Migrations are executed in the order defined by migration set, NOT lexicographically like in many migration tools,
version is only used to uniquely identify migration - this is by design.
{{</alert>}}

## Run migrations

Make sure you have `DATABASE_URL` set to your database.
If you don't have one you can use following `docker-compose.yml`:
```yaml
version: '3'

services:
  pg:
    image: postgres:12.3-alpine
    ports:
      - 5432:5432
    environment:
      POSTGRES_PASSWORD: secret
      POSTGRES_USER: krab
      POSTGRES_DB: krab

  pgweb:
    container_name: pgweb
    image: sosedoff/pgweb
    ports:
      - "8081:8081"
    environment:
      - DATABASE_URL=postgres://krab:secret@pg:5432/krab?sslmode=disable
    depends_on:
      - pg
```

then export it:

```shell
export DATABASE_URL="postgres://krab:secret@localhost:5432/krab?sslmode=disable"
```

now try and run the migration:

```
krab migrate up public
```

`public` is the reference name defined in `migration_set` in your file (commands are generated: `krab migrate up <migration_set>`).
Expected output should be:

```
OK  20230213_210359 create_animals
```

You can check the status of migrations:
```
krab migrate status public
```

Output:

```
✔ 20230213_210359 create_animals
```

Or you can check it with `psql`:
```
psql $DATABASE_URL
```

and see if migration was applied:
```psql
krab=# \dt
             List of relations
 Schema |       Name        | Type  | Owner
--------+-------------------+-------+-------
 public | animals           | table | krab
 public | schema_migrations | table | krab
(2 rows)

krab=# select * from schema_migrations;
     version     |          migrated_at
-----------------+-------------------------------
 20230213_210359 | 2023-02-13 21:22:13.554722+00
(1 row)
```
