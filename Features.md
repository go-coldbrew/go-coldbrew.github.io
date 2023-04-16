---
layout: default
title: "Features"
description: "Features of Coldbrew"
permalink: /features
---
# Features
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc} 

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
