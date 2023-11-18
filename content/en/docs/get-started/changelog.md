---
title: "Changelog"
description: "Changelog of latest changes."
lead: "For detailed view of changes refer to GitHub releases."
draft: false
images: []
menu:
  docs:
    parent: "get-started"
weight: 1400
toc: true
---

## 0.9.0

Features:

- Execute actions via UI
- Browse database list via UI
- Basic auth

## 0.8.0

- allow to disable transaction mode for actions
- test syntax proposal (only parsing)

## 0.7.0

- add fake generator for actions

## 0.6.2

- build with go 1.19

## 0.6.1

- add params to migration generator

## 0.6.0

- generate migration command

## 0.5.0

- rewrite parser from hclsimple to schema based decoding
- make defaults in columns DSL universal (raw SQL, no custom HCL type)

## 0.4.2

- add quote support for values
- add support for more defaults in column DSL

## 0.4.1

- migration status action
- be explicit about identifier quoting (`quote` -> `quote_ident`)
- improved CLI UI

## 0.4.0

- migration DSL (not only raw sql)
- custom schema for migration sets (new attribute)
- actions (new resource)
- resource arguments (parametrized commands)
- windows support
- small improvements for CLI output
- schema migration table contains timestamp now

## 0.3.1

- fixes issue with rolling back the same migration multiple times

## 0.3.0

- fixes issues with concurrent operations

## 0.2.4

- added docker

## 0.2.3

- added validations
- improved CLI output
- migration versions are required now

## Prereleases: 0.2.2 and below

- migrations

Do not use prerelease versions.

