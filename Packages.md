---
layout: default
title: Packages
description: "ColdBrew packages documentation"
permalink: /packages
---
# Packages
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc} 

## [Core]
The core module is the base module and provides the base implementation for ColdBrew. It works in conjunction with the other modules to provide the full functionality of Cold Brew.

Documentation can be found at [core-docs]

### [Config]
Coldbrew config package contains the configuration for the core package. It uses [envconfig] to load the configuration from the environment variables.

Documentation can be found at [config-docs]

---
[Core]: https://github.com/go-coldbrew/core/tree/main#readme
[core-docs]: https://pkg.go.dev/github.com/go-coldbrew/core
[Config]: https://github.com/go-coldbrew/core/tree/main/config#readme
[config-docs]: https://pkg.go.dev/github.com/go-coldbrew/core/config
