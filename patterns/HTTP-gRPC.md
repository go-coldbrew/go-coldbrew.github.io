---
layout: default
title: "HTTP from gRPC APIs"
parent: "Common Patterns"
---
## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

## Returning HTTP status codes from gRPC APIs

### Overview
gRPC APIs are defined using the [Protocol Buffers](https://developers.google.com/protocol-buffers/). gRPC provides a set of standard response messages that can be used to return errors from gRPC APIs. These messages are defined in the [google/rpc/status.proto].

```proto
// The `Status` type defines a logical error model that is suitable for
// different programming environments, including REST APIs and RPC APIs. It is
// used by [gRPC](https://github.com/grpc). Each `Status` message contains
// three pieces of data: error code, error message, and error details.
//
// You can find out more about this error model and how to work with it in the
// [API Design Guide](https://cloud.google.com/apis/design/errors).
message Status {
  // The status code, which should be an enum value of
  // [google.rpc.Code][google.rpc.Code].
  int32 code = 1;

  // A developer-facing error message, which should be in English. Any
  // user-facing error message should be localized and sent in the
  // [google.rpc.Status.details][google.rpc.Status.details] field, or localized
  // by the client.
  string message = 2;

  // A list of messages that carry the error details.  There is a common set of
  // message types for APIs to use.
  repeated google.protobuf.Any details = 3;
}
```
### gRPC status codes and HTTP status codes mapping

gRPC status codes can be easlity translated to HTTP status codes. The following table shows the mapping between the canonical error codes and HTTP status codes:

| gRPC status code      | HTTP status code |
| --------------------  | ---------------- |
| `OK`                  | 200              |
| `INVALID_ARGUMENT`    | 400              |
| `OUT_OF_RANGE`        | 400              |
| `FAILED_PRECONDITION` | 400              |
| `PERMISSION_DENIED`   | 403              |
| `NOT_FOUND`           | 404              |
| `ABORTED`             | 409              |
| `ALREADY_EXISTS`      | 409              |
| `RESOURCE_EXHAUSTED`  | 429              |
| `CANCELLED`           | 499              |
| `UNKNOWN`             | 500              |
| `UNIMPLEMENTED`       | 501              |
| `DEADLINE_EXCEEDED`   | 504              |

Full list of gRPC status codes can be found in the [google/rpc/code.proto] file.

### Returning errors from RPC

When the service returns an error from the rpc its mapped to http status code 500 by default. To return a different http status code, the service can return a `google.rpc.Status` message with the appropriate error code. The following example shows how to return a `google.rpc.Status` message with the `INVALID_ARGUMENT` error code:

```proto
    message GetBookRequest {
      string name = 1;
    }

    message GetBookResponse {
      Book book = 1;
    }

    service BookService {
      rpc GetBook(GetBookRequest) returns (GetBookResponse) {
        option (google.api.http) = {
          get: "/v1/{name=books/*}"
        };
      }
    }
```

```go

import (
  "google.golang.org/grpc/codes"
  "google.golang.org/grpc/status"
)

func (s *server) GetBook(ctx context.Context, req *pb.GetBookRequest) (*pb.Book, error) {
  if req.Name == "" {
    return nil, status.Errorf(codes.InvalidArgument, "Name argument is required")
  }
  ...
}

```

this will return a `google.rpc.Status` message with the `INVALID_ARGUMENT` error code in HTTP and gRPC:

```bash
$ grpcurl -plaintext -d '{"name": ""}' localhost:8080 BookService.GetBook
{
  "code": 3,
  "message": "Name argument is required"
}
```

```bash
$ curl -X GET localhost:8080/v1/books/
{
  "code": 3,
  "message": "Name argument is required"
}
```

### Returning additional error details

The `google.rpc.Status` message can also be used to return additional error details. The following example shows how to return a `google.rpc.Status` message with the `INVALID_ARGUMENT` error code and additional error details:

```proto
    message GetBookRequest {
      string name = 1;
    }

    message GetBookResponse {
      Book book = 1;
    }

    service BookService {
      rpc GetBook(GetBookRequest) returns (GetBookResponse) {
        option (google.api.http) = {
          get: "/v1/{name=books/*}"
        };
      }
    }
```

```go
import (
  "google.golang.org/grpc/codes"
  "google.golang.org/grpc/status"
  "google.golang.org/genproto/googleapis/rpc/errdetails"
)

func (s *server) GetBook(ctx context.Context, req *pb.GetBookRequest) (*pb.Book, error) {
  if req.Name == "" {
    st := status.New(codes.InvalidArgument, "Name argument is required")
    st, _ = st.WithDetails(&errdetails.BadRequest_FieldViolation{
      Field:       "name",
      Description: "Name argument is required",
    })
    return nil, st.Err()
  }
  ...
}
```

this will output

```bash
$ grpcurl -plaintext -d '{"name": ""}' localhost:8080 BookService.GetBook
{
  "code": 3,
  "message": "Name argument is required",
  "details": [
    {
      "@type": "type.googleapis.com/google.rpc.BadRequest",
      "fieldViolations": [
        {
          "field": "name",
          "description": "Name argument is required"
        }
      ]
    }
  ]
}
```

```bash
$ curl -X GET localhost:8080/v1/books/
{
  "code": 3,
  "message": "Name argument is required",
  "details": [
    {
      "@type": "type.googleapis.com/google.rpc.BadRequest",
      "fieldViolations": [
        {
          "field": "name",
          "description": "Name argument is required"
        }
      ]
    }
  ]
}
```

### Using Coldbrew errors package

All the above examples can be used with the [Coldbrew errors package] by using the functions `NewWithStatus/WrapWithStatus`

```go
import (
  "github.com/go-coldbrew/errors"
  "google.golang.org/grpc/codes"
  "google.golang.org/grpc/status"
  "google.golang.org/genproto/googleapis/rpc/errdetails"
)

func (s *server) GetBook(ctx context.Context, req *pb.GetBookRequest) (*pb.Book, error) {
  if req.Name == "" {
    st := status.New(codes.InvalidArgument, "Name argument is required")
    st, _ = st.WithDetails(&errdetails.BadRequest_FieldViolation{
      Field:       "name",
      Description: "Name argument is required",
    })
    return nil, errors.NewWithStatus("Name argument is required", st)
  }
  ...
}
```

using the `errors.WrapWithStatus` function has the same effect as `errors.Wrap` but it also sets the status code of the error to the status code of the `google.rpc.Status` message. Similarly, the `errors.NewWithStatus` function has the same effect as `errors.New` but it also sets the status code of the error to the status code of the `google.rpc.Status` message.

Coldbrew errors package also provides stack trace support for errrors, which can make debugging easier. For more information see Coldbrew [errors package].

---
[google/rpc/status.proto]: https://github.com/googleapis/googleapis/blob/master/google/rpc/status.proto
[google/rpc/code.proto]: https://github.com/googleapis/googleapis/blob/master/google/rpc/code.proto
[Coldbrew errors package]: https://pkg.go.dev/github.com/go-coldbrew/errors#NewWithStatus
[errors package]: https://pkg.go.dev/github.com/go-coldbrew/errors
[envconfig]: https://github.com/kelseyhightower/envconfig
[grpc-gateway]: https://grpc-ecosystem.github.io/grpc-gateway/
[Coldbrew]: https://docs.coldbrew.cloud
