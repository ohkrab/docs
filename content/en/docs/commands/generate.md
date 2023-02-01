---
title: "Generate"
description: "Generate command in CLI."
lead: ""
draft: false
images: []
toc: true
---

The `gen migration` command generates empty [migration]({{< ref "docs/resources/migration" >}}) file.

## Usage

```bash
krab gen migration -name [ref] [column1:type ... columnN:type]
```

### Options

- `ref` - name of the migration reference
- column:type - column definitions separated by space

#### Special arguments

Instead of specifying `column:type` you can use built-in syntax sugar:
- `id` - will generate id as a `bigint` column as an identity with primary key
- `timestamps` - will generate `created_at` and `updated_at` column pair with `timestamp with time zone` type

## Example

This will create new migration file `$KRAB_DIR/db/migrations/[YYYYMMDD]_[HHMMSS]create_maps.krab.hcl` with empty structure.

```bash
krab gen migration -name create_maps
```

File content:

```hcl
migration "create_maps" {
  version = "YYYMMDD_HHMMSS"

  up {
  }

  down {
  }
}
```

With arguments:

```bash
krab gen migration -name create_maps id name:varchar project_id:bigint timestamps
```

File content:

```hcl
migration "create_maps" {
  version = "YYYMMDD_HHMMSS"

  up {
    create_table "maps" {
      column "id" "bigint" {
        identity {}
      }
      column "name" "varchar" {}
      column "project_id" "bigint" {}
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
    drop_table "maps" {}
  }
}
```
