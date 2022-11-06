---
title: "Migration"
description: "Migration"
lead: ""
draft: false
images: []
menu:
  docs:
    parent: "resources"
toc: true
---

Migration resource is a single migrate operation.

```hcl
migration "<reference>" {
  version     = "<version>"
  transaction = true

  up {
	sql = "..."
  }

  down {
	sql = "..."
  }
}
```

- `<reference>` - is a migration reference name to use when connecting to other resources
- `<version>` - name that will be used to identify migration in the database, can only be digits, alphanumeric characters and underscores
- `up` / `down` - migration direction, contains SQL code to be executed
- `transaction (optional)` - specifies whether run migration in a transaction (default: `true`)

## DSL

Up/Down migration can use built-in DSL for Data Definition Language.

{{<alert>}}
DSL order matters, code will run in that order (`sql` attribute order also matters)
{{</alert>}}

Supported DDL:
- [CREATE TABLE](https://github.com/ohkrab/krab/blob/master/krab/type_ddl_create_table.go#L11)
- [DROP TABLE](https://github.com/ohkrab/krab/blob/master/krab/type_ddl_drop_table.go#L11)
- [CREATE INDEX](https://github.com/ohkrab/krab/blob/master/krab/type_ddl_create_index.go#L11)
- [DROP INDEX](https://github.com/ohkrab/krab/blob/master/krab/type_ddl_drop_index.go#L11)


## Example

```hcl
migration "create_tenants" {
  version = "20060102150405"

  up {
	sql = "CREATE TABLE tenants(name VARCHAR PRIMARY KEY)"
  }

  down {
	sql = "DROP TABLE tenants"
  }
}
```

## DSL example

```hcl
migration "create_categories" {
  version = "v1"

  up {
    create_table "categories" {
	  column "id" "bigint" {}
	  column "name" "varchar" { null = false }

	  primary_key { columns = ["id"] }
	}
  }

  down {
    drop_table "categories" {}
  }
}

migration "create_animals" {
  version = "v2"

  up {
	create_table "animals" {
      # "id" bigint GENERATED ALWAYS AS IDENTITY
	  column "id" "bigint" {
		identity {}
	  }

	  column "name" "varchar" { null = true }
	  
	  column "extinct" "boolean" {
	    null    = false
		default = "TRUE"
	  }

	  column "weight_kg" "int" { null = false }

      # "weight_g" int GENERATED ALWAYS AS (weight_kg * 1000) STORED
	  column "weight_g" "int" {
		generated {
		  as = "weight_kg * 1000" 
		}
	  }

	  column "category_id" "bigint" {
	    null = false
	  }

	  unique {
		columns = ["name"]
		include = ["weight_kg"]
	  }

	  primary_key {
	    columns = ["id"]
		include = ["name"]
	  }

      # CONSTRAINT "ensure_positive_weight" CHECK (weight_kg > 0)
	  check "ensure_positive_weight" {
	    expression = "weight_kg > 0"
	  }

	  foreign_key {
	    columns = ["category_id"]

		references "categories" {
		  columns = ["id"]

		  on_delete = "cascade"
		  on_update = "cascade"
		}
	  }
	}
  }

  down {
    drop_table "animals" {}
  }
}

migration_set "animals" {
  migrations = [
    migration.create_categories,
    migration.create_animals
  ]
}
```

## DSL index example

```hcl
migration "create_animals" {
  version = "v1"
  transaction = false

  up {
	create_table "animals" {
	  column "id" "bigint" {}

	  column "name" "varchar" {}
	  
	  column "extinct" "boolean" {}

	  column "weight_kg" "int" {}
	}

    # CREATE UNIQUE INDEX "idx_uniq_name" ON "animals" USING btree ("name") INCLUDE ("weight_kg")
	create_index "animals" "idx_uniq_name" {
	  unique  = true
	  columns = ["name"]
	  using   = "btree"
	  include = ["weight_kg"]
	}

    # CREATE INDEX CONCURRENTLY "idx_heavy_animals" ON "animals" ("weight_kg") WHERE (weight_kg > 5000)
	create_index "animals" "idx_heavy_animals" {
	  columns      = ["weight_kg"]
	  where        = "weight_kg > 5000"
	  concurrently = true
	}
  }

  down {
    drop_index "public.idx_uniq_name" {
	  cascade = true
	}

    drop_index "idx_heavy_animals" {
	  concurrently = true
	}

    drop_table "animals" {}
  }
}

migration_set "animals" {
  migrations = [
    migration.create_animals
  ]
}
```
