---
layout: default
title: Home
nav_order: 1
description: "Coldbrew is a Go library for creating cloud native applications."
permalink: /
---
# Quick and Simple microservices with ColdBrew
{: .fs-9 }

Coldbrew is a Go library for creating cloud native applications. It provides a set of libraries for building resilient, secure, and scalable applications. It also provides ready-made components for quickly creating cloud-native applications.
{: .fs-6 .fw-300 }

[Get started now](/getting-started){: .btn .btn-primary .fs-5 .mb-4 .mb-md-0 .mr-2 }
[View it on GitHub](https://github.com/go-coldbrew/){: .btn .fs-5 .mb-4 .mb-md-0 }

## Why ColdBrew ?

ColdBrew started out as [Orion], ColdBrew is the next evolution of Orion. It is a collection of libraries and ready-made components that we have built since 2016 to make our lives easier. We have open-sourced these libraries so that other developers can benefit from them.

## Who is using ColdBrew ?

ColdBrew is production ready and is used at [gojek](https://www.gojek.com/en-id/) serving billions of requests per day.

## Dont repeat yourself

ColdBrew integrates with all the popular libraries. We strongly believe in the [DRY] principle. Instead of repeating things we re-use already exisiting and popular open-source libraries, such as:

- [grpc] - grpc first
- [grpc-gateway] - RESTful APIs
- [prometheus] - Metrics
- [opentelemetry] - Tracing
- [jaeger] - Tracing
- [opentracing] - Tracing
- [hystrix-go] - Circuit Breaker
- [new relic] - Monitoring
- [sentry] - Error Reporting
- [go-grpc-middleware] - Middlewares: interceptors, helpers and utilities.


# Features

## Cloud Native

Coldbrew is grpc first, it use [grpc-gateway] for RESTful APIs and the service can be neatly packaged into a small alpine docker container.

## Resilient

Coldbrew integrates with [hystrix-go] for circuit breaker and [opentelemetry] for tracing. It also integrates with packages like [grpc_retry] and [go-grpc-middleware] for retries and middlewares.

## Scalable

Coldbrew services follow [12factor] and can be easily scaled horizontally using already implemented liveliness and readiness probes.

## Fast

Coldbrew services are fast, they are written in Go and are compiled to native binaries. They are also optimized for speed and memory usage.

## Ready to use

Coldbrew provides ready-made components for quickly creating cloud-native applications. Have a look at the [Getting Started] page to see how easy it is to create a new production ready service with all bells and whistles.

## Easy to use

ColdBrew is easy to use. The cookiecutter template is easy to use and the documentation is easy to follow. Allowing you to focus on business logic instead of boilerplate code.

## Easy to extend

ColdBrew is easy to extend. You can easily extend ColdBrew with your own custom components. All functionality is implemented over interfaces so you can easily swap out the default implementation with your own.

## Easy to integrate

ColdBrew is easy to integrate. You can easily integrate ColdBrew with your existing applications. All functionality is implemented in individual packages, so you can easily upgrade individual packages or pick and choose the packages you want to use.

## Easy to upgrade

ColdBrew is easy to upgrade. You can easily upgrade ColdBrew to the latest version. All functionality is implemented in individual packages, so you can easily upgrade individual packages or pick and choose the packages you want to use.

## Open Source

ColdBrew is completely open source. You can use it for free in your commercial projects.

---
[orion]: https://github.com/carousell/Orion
[grpc]:https://grpc.io/
[grpc-gateway]:https://grpc-ecosystem.github.io/grpc-gateway/
[prometheus]:https://prometheus.io/
[jaeger]:https://www.jaegertracing.io/
[opentracing]:https://opentracing.io/
[hystrix-go]: https://pkg.go.dev/github.com/afex/hystrix-go
[new relic]: https://newrelic.com/
[sentry]: https://sentry.io/
[go-grpc-middleware]: https://pkg.go.dev/github.com/grpc-ecosystem/go-grpc-middleware
[grpc_retry]: https://pkg.go.dev/github.com/grpc-ecosystem/go-grpc-middleware/retry
[opentelemetry]: https://opentelemetry.io/
[DRY]: https://en.wikipedia.org/wiki/Don%27t_repeat_yourself
[12factor]: https://12factor.net/
[getting started]: /getting-started/
