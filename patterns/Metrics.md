---
layout: default
title: "Metrics"
parent: "Common Patterns"
---
## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

## How Metrics Work in Coldbrew

Coldbrew uses [Prometheus](https://prometheus.io/) to collect service metrics. By Default, Coldbrew will expose a `/metrics` endpoint that will return the metrics in the [Prometheus exposition format](https://prometheus.io/docs/instrumenting/exposition_formats/) on the configured [HTTP port].

A collection of metrics are collected by default, including:
* Golang runtime metrics (e.g. memory usage, goroutine count, etc.)
* gRPC Client/Server metrics (e.g. request count, request duration, etc.)
* HTTP request metrics (e.g. request count, request duration, etc.)
* [Hystrix-go] circuit breaker metrics (e.g. request count, request duration, etc.) powered by [Hystrix Prometheus]

## How to Add Custom Metrics

You can add custom metrics to your service by using the [Prometheus Go client library] and registering them with the default Prometheus registry. For example:

```go
package main

import (
    "github.com/prometheus/client_golang/prometheus"
    "github.com/prometheus/client_golang/prometheus/promauto"
)

var (
    myCounter = promauto.NewCounter(prometheus.CounterOpts{
        Name: "my_counter",
        Help: "The total number of processed events",
    })
)

func main() {
    myCounter.Inc()
}
```

These metrics will be automatically collected and exposed by Coldbrew on the `/metrics` endpoint.

---
[HTTP port]: https://pkg.go.dev/github.com/go-coldbrew/core/config#readme-type-config
[Hystrix Prometheus]: https://pkg.go.dev/github.com/go-coldbrew/hystrixprometheus
[Hystrix-go]: https://github.com/afex/hystrix-go
[Prometheus Go client library]: https://github.com/prometheus/client_golang
