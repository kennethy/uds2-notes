## Chapter 5. APIs
- Request-response messaging: when a client sends a request message to the server, and the server replies with a response message.
- The response may be serialized in a human-readable format such as JSON or a binary format such as protocol buffers.
- The communication is synchronous when the client is blocked waiting for a response to arrive. Asynchronous is when the client may proceed for other tasks, and the server makes a respond by invoking a callback on the client.
- gRPC is a more compact way to allow internal servers to communicate with each other.
- Representation State Transfer (REST):
  - Stateless requests and each request contains all the necessary information required to process it
  - Responses could be marked as cacheable or non-cacheable. Cacheable response means the client can reused the response for a later, equivalent request.
  
### 5.1 HTTP
- Hypertext transfer protocol (HTTP) is a request-response protocol used to encode and transport information between a client and a server.
- HTTP transaction: when the client sends a request message to the server's API endpoint, and the server replies back with a response message.
- HTTP1
  - Uses a textual block of data that contains a start line, a set of headers, and an optional body
  - A stateless protocol that uses TCP for reliability gaurantees
  - Keeps a connection to a server open by default for reuse.
  - Head-of-line blocking (HOL) is when a request can't be issued until the response to the previous one has been received.
- HTTP1.1:
  - Allows some types of requests to be pipelined but still suffers from HOL blocking as a single slow response will block all the response after it.
- HTTP 2:
  - Uses a binary protocol rather than a textual one.
  - Supports multiplexing which allows multiple concurrent request-response transactions (streams) on the same connection.
  - Half of the most visited websites use HTTP2 by 2020.
- HTTP 3:
  - Latest iteration based on UDP and implements its own transport protocol
  - A packet loss over the TCP in HTTP2 could block all streams, whereas only one stream is affected in HTTP3.

### 5.2 Resources
- `https://www.example.com/products?sort-price`
  - `https` is the protocol.
  - `www.example.com` is the hostname.
  - `products` is the name of the resource.
  - `?sort=price` is the query string.

### 5.3 Request methods
- Most commonly used methods are `POST`, `GET`, `PUT`, AND `DELETE`.
- A safe method should not have any visible side effects.
- An idempotent method can be invoked numerous time and the outcome should be the same as it was executed once.

### 5.4 Response status codes
- 200 to 299 for success
  - 200 ok
  - 201 created
  - 202  accepted (and still being processed
- 300 to 399 for redirection
  - 301 permanently moved
  - 302 found but moved
  - 307 succeeds 302 and it doesn't allow the HTTP method to change.
  - 308 succeeds for 301 except it prohibits the change of the HTTP method.
- 400 to 499 for client errors
  - 400 bad request/bad client-side input
  - 401 unauthorized
  - 403 forbidden/authenticated but not allowed to access the resource.
  - 404 resource not found
- 500 to 599 for server errors
  - 500 internal server error
  - 502 bad gateway (downstream server unreachable or request failed)
  - 503 service unavailable (overload or in maintenance mode)

### 5.5 OpenAPI
- Specifies endpoints info in code and use code-gen for tooling

### 5.6 Evolution
- REST APIs can be versioned for compatability reasons.

### 5.7 Idempotency
- HTTP methods like `PUT` or `DELETE` should be treated as idempotent.
- An idempotency key is a unique identifier that identifies a request, and allows the server to detect whether it has been processed before. It may have a TTL.
  - Request identifiers should be updated in a transaction for atomicity.