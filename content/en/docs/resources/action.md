---
title: "Action"
description: "Custom SQL action configuration resource."
lead: ""
draft: false
images: []
toc: true
---

Action resource is a custom SQL operation that will be executed.

```hcl {lineNos=true}
action "<namespace>" "<name>" {
  arguments {
    ...
  }

  sql = "..."
}
```

- `<namespace>` - is a action namespace
- `<name>` - is a migration reference name to use when connecting to other resources
- `arguments` block (optional) - define arguments that can be used in `sql` as a variable, see [Resource arguments]({{< ref "docs/configuration/resource-arguments" >}}) for more details
- `sql` - code to be executed

## Arguments

These attributes can be used with arguments:

- `sql`


### Example

```hcl {lineNos=true}
action "view" "refresh" {
  arguments {
    arg "name" {
      description = "Materialized view to be refreshed"
    }
  }

  sql = "REFRESH MATERIALIZED VIEW {{ .Args.name | quote_ident }}"
}
```


## Fakes

You can generate fake data with `fake` function template.

```hcl {lineNos=true}
action "seed" "all" {
  sql = <<-SQL
  INSERT INTO companies (name) VALUES ({{ fake "Company.Name" | quote }})
  SQL
}
```
