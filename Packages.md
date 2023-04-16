---
layout: default
title: Packages
nav_order: 3
description: "ColdBrew packages documentation"
permalink: /packages
---
# Packages

## [Core](/core)
Coldbrew core package provides the glue for building resilient, secure, and scalable applications. It uses all the ColdBrew libraries for logging, metrics, tracing, and error handling. ColdBrew is grpc first, it use [grpc-gateway] RESTful APIs

Documentation can be found at [core-docs]

### [Config](/core-config)
Coldbrew config package contains the configuration for the core package. It uses [viper] to load the configuration from the environment variables.

Documentation can be found at [config-docs]

---
[grpc-gateway]:https://grpc-ecosystem.github.io/grpc-gateway/
[core-docs]:https://pkg.go.dev/github.com/go-coldbrew/core
[config-docs]:https://pkg.go.dev/github.com/go-coldbrew/core/config

[grpc]:https://grpc.io/
[prometheus]:https://prometheus.io/
[jaeger]:https://www.jaegertracing.io/
[opentracing]:https://opentracing.io/
[hystrix-go]: https://pkg.go.dev/github.com/afex/hystrix-go
[NewRelic]: https://newrelic.com/
[go-grpc-middleware]: https://pkg.go.dev/github.com/grpc-ecosystem/go-grpc-middleware

