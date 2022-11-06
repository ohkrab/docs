---
title: "Resource arguments"
description: "Usage of resource arguments and built-in functions."
lead: ""
draft: false
images: []
menu:
  docs:
    parent: "configuration"
weight: 30
toc: true
---

Some resources accept arguments block that allows to parametrize command.

Arguments blocks are optional and can expand configuration in a flexible ways.

```hcl
resource ["label" ["label"...]] {
  arguments {
    arg "one" {
      description = "Some argument description"
    }

    arg "two" {
      description = "Argument with specified type"
      type        = "string" # default
    }
  }
}
```

- `arg "name"` - argument name
- `description` - summary of an argument 
- `type` - currently can only be a `string` type
- arguments are required to pass into command

### Arguments usage

Krab uses Go lang templates to replace values. For full documentation refer to [official documentation](https://pkg.go.dev/text/template).
For arguments support refer to specific resource documentation.

Arguments are not quoted by default unless stated otherwise ⚠️.

Example:

```
sql = "CREATE SCHEMA {{ quote_ident .Args.name }}"
```

All arguments must be prefixed with `.Args`.


### Built-in functions

There are built-in functions that allow to operate on arguments before final template is rendered.

- `quote_ident` - quotes identifiers in database, for example: table/column names 
- `quote` - quotes values in database

