---
title: "Action"
description: "Action"
lead: ""
draft: false
images: []
menu:
  docs:
    parent: "commands"
    identifier: "commands-action"
toc: true
---

Actions are genarated from configuration and grouped by namespace.

## Usage

```bash
krab action namespace name [arguments]
```

## Example

For [view refresh action]({{< ref "docs/configuration/resources/action#example" >}}) you would use:

```bash
krab action view refresh -name my-view-name
```

