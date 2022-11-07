---
title: "Migrate status"
description: "Migrate status command in CLI."
lead: ""
draft: false
images: []
toc: true
---

The `migrate status` command displays currently applied/pending migrations.

## Usage

```bash
krab migrate status [set]
```

### Options

- `set` - name of the set to migrate.

### Example output

```bash
krab migrate status animals

✔ v1 create_animals
✔ v2 create_animals_view
- v3 seed_animals
```

