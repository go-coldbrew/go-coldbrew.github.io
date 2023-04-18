---
layout: default
title: Common Patterns
description: "A collection of common patterns for the ColdBrew"
permalink: /patterns
---
# Common Patterns
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

## Introduction

This page is a collection of common patterns for the ColdBrew. If you have a pattern you would like to share, please [open an issue](https://github.com/go-coldbrew/docs.coldbrew.cloud/issues)

## Context aware logs

In any service there a set of common items that you want to log with every log message. These items are usually things like the request-id, trace-id, user-id, etc. It is useful to have these items in the log message so that you can filter on them in your log aggregation system. This is especially useful when you have multiple points of logs and you want to be able to trace a request through the system.

ColdBrew provides a way to add these items to the log message using the `loggers.AddToLogContext` function. This function takes a `context.Context` and `key, value`. AddToLogContext adds log fields to context. Any info added here will be added to all logs using this context.

```go
import (
    "github.com/go-coldbrew/log"
    "github.com/go-coldbrew/log/loggers"
)

func handler(w http.ResponseWriter, r *http.Request) {
    ctx = r.Context()
    ctx = loggers.AddToLogContext(ctx, "request-id", "1234")
    ctx = loggers.AddToLogContext(ctx, "trace-id", "5678")
    ctx = loggers.AddToLogContext(ctx, "user-id", "abcd")
    helloWorld(ctx)
}

func helloWorld(ctx context.Context) {
    log.Info(ctx, "Hello World")
}
```

Will output

```
{"level":"info","msg":"Hello World","request-id":"1234","trace-id":"5678","user-id":"abcd","@timestamp":"2020-05-04T15:04:05.000Z"}
```

## Trace ID propagation in logs

When you have multiple services, it is useful to be able to trace a request through the system. This is especially useful when you have a request that spans multiple services and you want to be able to see the logs for each service in the context of the request. Having a propagating trace-id is a good way to do this.

ColdBrew makes it easier to propagate trace ids by providing the [TraceId interceptor] which will automatically add the trace-id to the log context when the request specifies a `trace_id` field in the proto request.

```proto
message HelloRequest {
    string trace_id = 1;
    string msg = 2;
}

service HelloService {
    rpc Hello(HelloRequest) returns (HelloResponse) {
        option (google.api.http) = {
            get: "/hello"
        };
    }
}
```

## Adding Tracing to your functions

ColdBrew provides a way to add tracing to your functions using the [go-coldbrew/tracing] package. The Package provides function like `NewInternalSpan/NewExternalSpan/NewDatabaseSpan` which will create a new span and add it to the context.

Make sure you use the context returned from the `NewInternalSpan/NewExternalSpan/NewDatabaseSpan` functions. This is because the span is added to the context. If you don't use the context returned from the function, new spans will not be add at the correct place in the trace.

You can also add tags to the span using the `SetTag/SetQuery/SetError` function. These tags will be added to the span and will be visible in the trace view of your tracing system (e.g. New Relic / Opentelemetry).

Adding `defer span.End()` will make sure that the span will end when the function returns. If you don't end the span, it may never be sent to the tracing system and/or have the wrong duration.

```go
import (
    "github.com/go-coldbrew/tracing"
    "context"
)

func myFunction1(ctx context.Context) {
    span, ctx := tracing.NewInternalSpan(ctx, "myFunction1") // start a new span for this function
    defer span.End() // end the span when the function returns
    span.SetTag("myTag", "myValue") // add a tag to the span to help identify it in the trace view of your tracing system (e.g. Jaeger)
    myFunction2(ctx)
}

func myFunction2(ctx context.Context) {
    span, ctx := tracing.NewInternalSpan(ctx, "myFunction2") // start a new span for this function
    defer span.End() // end the span when the function returns
    helloWorld(ctx)
}

func helloWorld(ctx context.Context) {
    span, ctx := tracing.NewInternalSpan(ctx, "helloWorld") // start a new span for this function
    defer span.End() // end the span when the function returns
    log.Info(ctx, "Hello World")
}

func main() {
    ctx := context.Background()
    myFunction1(ctx)
}
```

---

[TraceId interceptor]: https://pkg.go.dev/github.com/go-coldbrew/interceptors#TraceIdInterceptor
[go-coldbrew/tracing]: https://pkg.go.dev/github.com/go-coldbrew/tracing
