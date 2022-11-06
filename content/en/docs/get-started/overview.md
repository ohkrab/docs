---
title: "Overview"
description: "Overview"
lead: ""
images: []
menu:
  docs:
    parent: "get-started"
    identifier: "get-started-overview"
weight: 110
toc: true
---

Krab documentation consists of the following chapters:

- Get started - things to get you set up and running
- Configuration - resources to use in the configuration files
- Functions - functions to use in the configuration files
- Commands - manual for CLI

## Glossary

- **configuration file** - file with `.krab.hcl` extension
- **resource** - block of code of a given type in a configuration file (e.g. `migration` resource, `migration_set` resource etc.)
- **function** - built-in function that is available to use in config files
- **command** - CLI command that executes built-in action like `migrate up`
- **reference** - identifiable name of a resource, must be unique within a given type (`migration "that_is_reference" {...}`)

## Other places

- [Repository with examples](https://github.com/ohkrab/examples)
- [GitHub releases](https://github.com/ohkrab/krab/releases)
- [Docker Hub](https://hub.docker.com/orgs/ohkrab/repositories)
