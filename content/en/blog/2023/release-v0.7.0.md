---
title: "Release v0.7.0 notes"
description: "Fake generators as custom functions for Oh, Krab! templating engine."
excerpt: "Latest addition brings fake data generators into templates."
date: 2023-02-19T21:46:42+0100
draft: false
weight: 50
images: []
categories: ["News"]
tags: ["release", "krab", "faker", "seeds"]
contributors: ["qbart"]
pinned: false
homepage: false
---

Latest release adds additional functions to Krab's built-in templating engine (provided by Go standard library).
At this point there were only two functions

- `quote` - for escaping values
- `quote_indent` - for escaping PostgreSQL identifiers

Use of such function can be demostrated with a simple action:

```hcl
action "view" "refresh" {
  arguments {
    arg "name" {
      description = "Materialized view to be refreshed"
    }
  }

  sql = "REFRESH MATERIALIZED VIEW {{ .Args.name | quote_ident }}"
}
```

which allows to refresh materiazlied views from CLI by specifing name argument and propery escaping it in SQL:

```
krab action view refresh -name cached_animals
```

The newest version extends Krab's template functions library with fake data generator, think of this as a fixture / faker.
To provide this functionallity [jaswdr/faker](https://github.com/jaswdr/faker) library is used.
Currently supported generators are:

- `Address.Country`
- `Address.CountryCode`
- `Color.Name`
- `Color.Hex`
- `Color.RGB`
- `Company.Name`
- `Emoji.Emoji`
- `Emoji.EmojiCode`
- `File.Path`
- `File.Extension`
- `File.FilenameWithExtension`
- `File.MimeType`
- `Food.Fruit`
- `Food.Vegetable`
- `Hash.MD5`
- `Hash.SHA256`
- `Internet.Domain`
- `Internet.Email`
- `Internet.Ipv4`
- `Internet.Ipv6`
- `Internet.MacAddress`
- `Internet.SafeEmail`
- `Internet.Slug`
- `Internet.URL`
- `Internet.UserAgent`
- `Lorem.Paragraph`
- `Lorem.Word`
- `Person.FirstName`
- `Person.LastName`
- `Person.Name`
- `Time.ISO8601`
- `Time.Timezone`

For implementation details see [here](https://github.com/ohkrab/krab/blob/8e1e35da75e8103a731e3749f474153ccdd10665/krabtpl/fake.go).
If you need more, you can [open discussion](https://github.com/ohkrab/krab/discussions/categories/ideas) about it.

Consider following action:

```hcl
action "seed" "company" {
  sql = <<-SQL
  INSERT INTO companies (name) VALUES ({{ fake "Company.Name" | quote }})
  SQL
}
```

All you need to do right now is to run the command:

```
krab action seed company
```

And that's all. Fakes are still at early stage, when more features are added they will be integrated into more functionalities.
