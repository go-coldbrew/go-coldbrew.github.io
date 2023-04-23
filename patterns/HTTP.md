---
layout: default
title: "HTTP APIs"
parent: "Common Patterns"
---
## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

## Content-Type

Coldbrew supports multiple content-types for requests and responses. The default content-type is `application/json`. The following content-types are supported by default:

- `application/json`
- `application/proto`
- `application/protobuf`

Lets assume the following proto definition:

```proto
message EchoRequest{
  string msg = 1;
}
message EchoResponse{
  string msg = 1;
}

service MySvc {
  rpc Echo(EchoRequest) returns (EchoResponse) {
    option (google.api.http) = {
      post: "/api/v1/example/echo"
      body: "*"
    };
    option (grpc.gateway.protoc_gen_openapiv2.options.openapiv2_operation) = {
      summary: "Echo endpoint"
      description: "Provides an echo reply endpoint."
      tags: "echo"
    };
  }
}
```

and the following service implementation:

```go
// Echo returns the message with the prefix added
func (s *svc) Echo(_ context.Context, req *proto.EchoRequest) (*proto.EchoResponse, error) {
	return &proto.EchoResponse{
		Msg: fmt.Sprintf("%s: %s", "echo", req.GetMsg()),
	}, nil
}
```

{: .note}
when *Content-Type* or *Accept* is not specified in the request header, the default content-type of `application/json` is used.

### JSON request, JSON response

When we send a curl call to the endpoint, we get the following response:

```bash
 $ curl -X POST -d '{"msg":"hello"}' -i http://127.0.0.1:9091/api/v1/example/echo
HTTP/1.1 200 OK
Content-Type: application/json
Grpc-Metadata-Content-Type: application/grpc
Vary: Accept-Encoding
Date: Sun, 23 Apr 2023 13:42:37 GMT
Content-Length: 20

{"msg":"echo: hello"}%
```

### JSON request, Proto response

We can send a proto request and get a proto response by specifying the *Accept* header:

```bash
curl -X POST -H 'Accept: application/proto' -d '{"msg":"hello"}' -i http://127.0.0.1:9091/api/v1/example/echo
HTTP/1.1 200 OK
Content-Type: application/octet-stream
Grpc-Metadata-Content-Type: application/grpc
Vary: Accept-Encoding
Date: Sun, 23 Apr 2023 13:46:47 GMT
Content-Length: 12


echo: hello%
```

### Proto request, Proto response

We can send a proto request and get a JSON response by specifying the *Content-Type* header:

```bash
$ echo 'msg: "proto message"' | protoc --encode=EchoRequest proto/app.proto | curl -sS -X POST --data-binary @- -H 'Content-Type: application/proto' -i http://127.0.0.1:9091/api/v1/example/echo
HTTP/1.1 200 OK
Content-Type: application/octet-stream
Grpc-Metadata-Content-Type: application/grpc
Vary: Accept-Encoding
Date: Sun, 23 Apr 2023 14:07:38 GMT
Content-Length: 20


echo: proto message%

```
