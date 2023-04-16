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

## [Log]
log provides a minimal interface for structured logging in services. It provides a simple interface to log errors, warnings, info and debug messages. It also provides a mechanism to add contextual information to logs. We provide implementations for gokit, logrus and zap.

Documentation can be found at [log-docs]

## [Interceptors]
Interceptors provides a common set of reusable interceptors for grpc services

Documentation can be found at [interceptor-docs]

## [Errors]
errors provides an implementation of golang error with stack strace information attached to it, the error objects created by this package are compatible with https://golang.org/pkg/errors/

Documentation can be found at [errors-docs]

### [Notifier]
notifier provides notifier services for error reporting (airbrake, bugsnag, rollbar, sentry). Notifier replies on [Errors] package to get the stack trace information.

Documentation can be found at [notifier-docs]

---
[Core]: https://github.com/go-coldbrew/core/tree/main#readme
[core-docs]: https://pkg.go.dev/github.com/go-coldbrew/core
[Config]: https://github.com/go-coldbrew/core/tree/main/config#readme
[config-docs]: https://pkg.go.dev/github.com/go-coldbrew/core/config
[Log]: https://github.com/go-coldbrew/log/tree/main#readme
[log-docs]: https://pkg.go.dev/github.com/go-coldbrew/log
[Interceptors]: https://github.com/go-coldbrew/interceptors/tree/main#readme
[interceptor-docs]: https://pkg.go.dev/github.com/go-coldbrew/interceptors
[Errors]: https://github.com/go-coldbrew/errors/tree/main#readme
[errors-docs]: https://pkg.go.dev/github.com/go-coldbrew/errors
[Notifier]: https://github.com/go-coldbrew/errors/tree/main/notifier#readme
[notifier-docs]: https://pkg.go.dev/github.com/go-coldbrew/errors/notifier
