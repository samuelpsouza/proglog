# proglog

A distributed services project built while studying **[Distributed Services with Go](https://pragprog.com/titles/tjgo/distributed-services-with-go/)** by Travis Jeffery.

## About

This project implements the examples from the book, progressively building a distributed log service from the ground up. The goal is to understand how to architect and build distributed systems using Go.

## Project Structure

```
proglog/
├── cmd/
│   └── server/
│       └── main.go          # Entry point for the server
├── internal/
│   └── server/
│       ├── http.go          # HTTP server and request handlers
│       └── log.go           # In-memory log implementation
├── go.mod
├── go.sum
└── README.md
```

## Current Implementation

### Log Service
- **In-memory log storage** with thread-safe operations using mutexes
- **Record structure** with Value and Offset fields
- **Append operation** to add records to the log
- **Read operation** to retrieve records by offset

### HTTP API
- **POST** `/` - Produce (append a record to the log)
  - Request: `{"record": {"value": "<base64-encoded-value>"}}`
  - Response: `{"offset": <uint64>}`
  
- **GET** `/` - Consume (read a record from the log)
  - Request: `{"offset": <uint64>}`
  - Response: `{"record": {"value": "<base64-encoded-value>", "offset": <uint64>}}`

### Server
- HTTP server running on port `:8080` by default
- Uses [Gorilla Mux](https://github.com/gorilla/mux) for routing

## Getting Started

### Prerequisites
- Go 1.25.5 or later

### Installation

1. Clone the repository:
```bash
git clone https://github.com/samuelpsouza/proglog.git
cd proglog
```

2. Download dependencies:
```bash
go mod download
```

### Running the Server

```bash
go run ./cmd/server
```

The server will start on `http://localhost:8080`

### Example Usage

Produce a record:
```bash
curl -X POST http://localhost:8080 \
  -H "Content-Type: application/json" \
  -d '{"record":{"value":"aGVsbG8="}}'
```

Consume a record:
```bash
curl -X GET http://localhost:8080 \
  -H "Content-Type: application/json" \
  -d '{"offset":0}'
```

## Dependencies

- **gorilla/mux** - HTTP routing

## Learning Resources

This project is based on the book *Distributed Services with Go* by Travis Jeffery, which covers:
- Building distributed services from scratch
- Service architecture and API design
- Consensus and replication
- Service discovery and load balancing
- And more!

## License

This project is created for learning purposes.
