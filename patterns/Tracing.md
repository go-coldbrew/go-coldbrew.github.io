---
layout: default
title: "Tracing"
parent: "Common Patterns"
---
## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

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
    // do something
    myFunction2(ctx)
    // do something
}

func myFunction2(ctx context.Context) {
    span, ctx := tracing.NewInternalSpan(ctx, "myFunction2") // start a new span for this function
    defer span.End() // end the span when the function returns
    // do something
    helloWorld(ctx)
    // do something
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

## Adding Tracing to your gRPC services

### Adding tracing to your gRPC server

When you create a new service with [ColdBrew cookiecutter] it will automatically add tracing (New Relic / Opentelemetry) to your gRPC services. This is done by adding the [interceptors] to your gRPC server.

{: .note .note-info }
To disable coldbrew provided interceptors you can call the function [UseColdBrewServcerInterceptors].

If you want to add interceptors to your gRPC server, you can use the [Default Interceptors] from [interceptors] package to add the ColdBrew interceptors to your gRPC server.

Example:

```go
import (
    "context"
    "github.com/go-coldbrew/interceptors"
    "github.com/go-coldbrew/log"
    "google.golang.org/grpc"
)

func main() {
    server := grpc.NewServer(
        // Add the ColdBrew interceptors to your gRPC server to add tracing/metrics to your gRPC server calls
        grpc.ChainUnaryInterceptor(interceptors.DefaultInterceptors()...),
    )
    pb.RegisterHelloWorldServer(server, &HelloWorldServer{})
    if err := server.Serve(lis); err != nil {
        log.Fatal(context.Background(), err)
    }
}
```

### Adding tracing to your gRPC client

ColdBrew provides gRPC client interceptors to add tracing/metrics to your gRPC client. You can add [Default Client Interceptors] which are a collection of interceptors provided by ColdBrew, or you can add your own interceptors.

Example:

```go
import (
    "github.com/go-coldbrew/interceptors"
    "github.com/go-coldbrew/log"
    "google.golang.org/grpc"
)

func main() {
    ctx := context.Background()
    conn, err := grpc.Dial(
        "localhost:8080",
        grpc.WithInsecure(),
        // Add the ColdBrew interceptors to your gRPC client to add tracing/metrics to your gRPC client calls
        grpc.WithChainUnaryInterceptor(interceptors.DefaultClientInterceptors()...),
    )
    if err != nil {
        log.Fatal(ctx, err)
    }
    defer conn.Close()
    client := pb.NewHelloWorldClient(conn)
    resp, err := client.HelloWorld(ctx, &pb.HelloWorldRequest{})
    if err != nil {
        log.Fatal(ctx, err)
    }
    log.Info(ctx, resp)
}
```

---

[TraceId interceptor]: https://pkg.go.dev/github.com/go-coldbrew/interceptors#TraceIdInterceptor
[go-coldbrew/tracing]: https://pkg.go.dev/github.com/go-coldbrew/tracing
[ColdBrew cookiecutter]: /getting-started
[interceptors]: https://pkg.go.dev/github.com/go-coldbrew/interceptors
[UseColdBrewServcerInterceptors]: https://pkg.go.dev/github.com/go-coldbrew/interceptors#UseColdBrewServerInterceptors
[Default Client Interceptors]: https://pkg.go.dev/github.com/go-coldbrew/interceptors#DefaultClientInterceptors
[Default Interceptors]: https://pkg.go.dev/github.com/go-coldbrew/interceptors#DefaultInterceptors
