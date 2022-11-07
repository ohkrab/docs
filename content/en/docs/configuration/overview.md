---
title: "Overview"
description: "Overview of available configuration."
lead: ""
draft: false
images: []
toc: true
weight: 2100
---

Krab by default will load and parse all the configuration files at `KRAB_DIR` path.
File names should end with `.krab.hcl` extension, otherwise they won't be loaded.

Every command that touches database is required to provide `DATABASE_URL` environment variable.
