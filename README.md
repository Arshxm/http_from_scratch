# http_from_scratch

HTTP/1.1 server implementation from scratch in Go for Deeper Understanding of both protocol (RFC 9110, 9112) and GO(Lang).

## Features

- HTTP/1.1 request parsing with state machine
- Header parsing and management
- Response writing with status codes
- Chunked transfer encoding support
- Concurrent request handling

## Requirements

Go 1.25.4 or later

## Installation

```bash
go build -o httpserver ./cmd/httpserver
```

## Usage

```go
package main

import (
    "http_tcp/internal/response"
    "http_tcp/internal/request"
    "http_tcp/internal/server"
)

func main() {
    s, err := server.Serve(42069, func(w *response.Writer, req *request.Request) {
        h := response.GetDefaultHeaders(0)
        body := []byte("<html><body>Hello</body></html>")
        h.Replace("Content-length", fmt.Sprintf("%d", len(body)))
        h.Replace("Content-type", "text/html")
        
        w.WriteStatusLine(response.StatusOK)
        w.WriteHeaders(*h)
        w.WriteBody(body)
    })
    if err != nil {
        log.Fatal(err)
    }
    defer s.Close()
}
```

Run the server:

```bash
go run ./cmd/httpserver
```

## Project Structure

```
.
├── cmd/
│   ├── httpserver/     # HTTP server application
│   └── tcplistener/    # TCP listener for testing
├── internal/
│   ├── headers/        # HTTP header parsing and management
│   ├── request/        # HTTP request parsing
│   ├── response/       # HTTP response writing
│   └── server.go       # Server implementation
└── go.mod
```

## Testing

```bash
go test ./...


