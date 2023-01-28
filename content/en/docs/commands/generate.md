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
krab gen migration -name [ref]
```

### Options

- `ref` - name of the migration reference

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


