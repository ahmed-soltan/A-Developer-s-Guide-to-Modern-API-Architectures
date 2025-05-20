# A Developer's Guide to Modern API Architectures

## Introduction

In the ever-evolving world of web development, choosing the right API design can be the difference between a smooth developer experience and a tangled mess of code. From the simplicity of REST to the performance of gRPC, and the type-safety of tRPC to the flexibility of GraphQL — each style brings its own strengths and trade-offs. In this article, we'll break down five popular API architectures — REST, RESTful APIs, GraphQL, tRPC, and gRPC — to help you understand what they are, how they work, and when to use each one.

---

## What is REST?

### Definition
REST stands for Representational State Transfer, it is an architectural style for designing networked application, it's invented by Roy Fielding in his 2000 PhD dissertation. REST has Six Principles:

- Uniform Interface
- Statelessness
- Cacheability
- Client-Server Architecture
- Layered System
- Code-on-Demand

### Let's Talk about each one separately

#### 1. Uniform Interface
We Can Categorize it into 3 main parts:

- **Resource Identification**: REST API uses Uniform Resources Identifiers (URIs) to address resources. For example, a user can be identified by URI like `api.example.com/users/123`, where 123 is the user Id.
- **Self-Description Message**: The server includes headers like Content-Type in the response, which is needed to correctly parse the data.
- **HATEOAS**: The server response might include hypermedia link like `api.site.com/users/123`, guiding the client on how to perform further actions.

##### What is HATEOAS?

HATEOAS is a constraint of REST architecture where a server response not only includes the requested data but also hyperlinks (URLs) to related actions or resources. This allows the client to navigate the API dynamically based on the server's responses — without needing to hardcode endpoint paths.

In other words, the server drives the application's state transitions by including links (hypermedia) in the responses.

###### Example: Without HATEOAS
```http
GET /users
[ { "id": 1, "name": "Alice" }]
```
*The client must hardcode the next step: `/users/1`*

###### Example: With HATEOAS
```json
GET /users
[ {
    "id": 1,
    "name": "Alice",
    "links": {
      "self": "/users/1",
      "orders": "/users/1/orders"
    }
}]
```
*The client follows links provided by the server.*

#### 2. Statelessness: How It Works?
- The server does not store client state between requests
- Each request is independent 

> **Common Question**: What about authorized requests? Session indicates that we have a state on the server.
>
> **Answer**: Yes, but each request is still independent of the others, each has session stored in HTTP request headers or cookies so using session does not violate statelessness principle.

#### 3. Cacheability
- Responses from the server should be labeled as cacheable or non-cacheable
- Cache control headers like `cache-control` are used to determine the caching behavior
- Cache can be implemented in multiple levels: in Browser, in a proxy server, in CDN, etc.

#### 4. Client-Server Architecture
The client server design keeps website view separated from data. This makes it easier to use different views in multiple devices. It also helps websites to adapt and grow without everything getting mixed up.

#### 5. Layered System
When you are using the internet, you can't know whether you are talking directly to the website or if there is something in the middle. If there is a middleman like proxy or load balancer it won't mess up your communication and you don't have to change anything on your computer.

#### 6. Code on Demand
Servers can extend or customize the functionality of the client by transferring executable code.

## REST vs RESTful API

### REST (Representational State Transfer) 🔹
- **Definition**: REST is an architectural style for designing networked applications
- REST has 6 principles
- REST is the theory or guidelines

### RESTful 🔹
- **Definition**: RESTful refers to applications, services, or APIs that implement the principles of REST
- **Usage**: When an API follows the REST architecture (e.g., uses HTTP methods like GET, POST, PUT, DELETE appropriately), it is called a RESTful API
- RESTful is the implementation of REST principles

### When To Use REST?
- You're building simple CRUD operations
- You want a standard, well-supported protocol
- You're exposing a public API (e.g., Twitter API, GitHub API)
- You want clear versioning and caching with HTTP
- You have multiple clients (web, mobile, etc.) consuming the same endpoints

---

## What is GraphQL?

### Definition
GraphQL is a query language and runtime for APIs. It allows you to query the server to get the data you need and only receive the data in response, rather than a fixed data set just like REST APIs.

With GraphQL the client specifies the data structure they want, and the server responds with that exact data. This approach fixes problems like over-fetching and under-fetching.

### Advantages
- **Reduce Resources Consumption**: 
  - Get only the data needed in a single query
  - Solves over-fetching and under-fetching problems
- **Usability**: 
  - No need to memorize multiple endpoints
  - Single endpoint with described data needs
- **Static Analysis**: 
  - Schema-based system
  - Predictable backend responses
  - No additional validation needed
- **Automatically Generated Documentation**: 
  - Solves outdated documentation problems
- **Rich Tooling**: 
  - Apollo Client (The Best)
  - Relay
  - And more

### Disadvantages
- **Learning Curve**: 
  - Relatively new compared to REST
  - Less familiar to many developers
- **Bundle Size**: 
  - Requires more code
  - Results in larger bundle size
- **Network-Cache Level**: 
  - Uses POST request to single endpoint
  - Loses browser caching benefits
- **Security Issues**: 
  - Vulnerable to DoS attacks
  - Flexibility creates security challenges
- **Complex Backend Implementation**: 
  - Difficult to create performant queries
  - Challenging to maintain flexibility

### Use Cases

#### 1. Loosely Coupled Frontend and Backend Teams
> When the frontend and backend are represented by a single fullstack team, or the teams work closely together, GraphQL's flexibility might not be necessary since endpoints can be customized as needed.

#### 2. Multiple Clients
> When an API serves multiple clients, flexibility becomes crucial

#### 3. Mobile Clients
> The impact of under-fetching and over-fetching of data can be particularly significant for mobile applications

#### 4. Applications with Complex Data Relationships
> GraphQL excels at handling intricate data relationships and nested queries

---

## What is RPC?

### Definition
RPC stands for Remote Procedure Call. It is a protocol where a client can call functions (procedures) on a remote server as if it were a local function — hiding the complexity of networking behind function calls.

Unlike REST or GraphQL, which are resource-based, RPC is action-based: you call a function like `getUser()` or `createOrder()` instead of `GET /users`.
RPC is commonly used in microservices, high-performance systems, and strongly typed architectures.

There are many implementations of RPC, but here we'll focus on two modern ones: gRPC and tRPC.

---

## gRPC – Remote Procedure Calls with Protocol Buffers

### Definition
gRPC (Google Remote Procedure Call) is an open-source RPC framework developed by Google. It uses Protocol Buffers (protobuf) as its interface definition language and supports multiple languages (Go, Java, C++, Python, etc.).

### Advantages
- **High Performance**: Uses HTTP/2 and binary protocol for speed
- **Streaming Support**: Client, server, and bidirectional streaming
- **Strongly Typed Contracts**: via .proto files
- **Cross-language Support**: Ideal for microservices in polyglot environments
- **Auto-generated Code**: from protobuf definitions

### Disadvantages
- **Harder to Debug**: Binary format makes it harder to inspect in-browser
- **Steeper Learning Curve**: Requires knowledge of protobuf and setup
- **Limited Browser Support**: Direct use from browsers is not easy (needs gRPC-Web)

### Use Cases
- Microservices communication
- Real-time apps (chat, live data)
- Internal systems where speed and type safety matter
- IoT and low-latency systems

---

## tRPC – Type-Safe RPC for TypeScript Projects

### Definition
tRPC is a modern RPC framework that allows you to build fully type-safe APIs without writing a separate schema. It's designed for TypeScript projects, typically used in fullstack applications where both client and server are written in TS (like Next.js apps).

### Advantages
- **End-to-end Type Safety**: Client + server share types
- **No Schema Writing**: Unlike GraphQL or OpenAPI
- **Fast Development**: Excellent DX
- **Perfect for Monorepos**: Great for fullstack TS projects
- **Modern Framework Integration**: Especially with Next.js

### Disadvantages
- **TypeScript Only**: Not cross-language
- **Not for Public APIs**: Tight coupling between client/server
- **Limited Scale**: Not ideal for microservices or distributed systems

### Use Cases
- Fullstack TypeScript apps (Next.js, Remix)
- Internal tools and dashboards
- Rapid prototyping for startups
- Monolithic apps with tight client-server integration

---

## 🧾 Choosing the Right API Architecture

| Criteria | REST | GraphQL | tRPC | gRPC |
|----------|------|---------|------|------|
| Simplicity | ✅ | ⚠️ Moderate | ✅ | ❌ Steep learning |
| Type Safety | ❌ | ⚠️ With Codegen | ✅ | ✅ (via .proto) |
| Performance | ⚠️ | ⚠️ Moderate | ✅ | ✅✅ High |
| Caching Support | ✅ | ❌ | ✅ | ⚠️ Manual needed |
| Real-time Support | ❌ | ⚠️ Workarounds | ⚠️ Experimental | ✅✅ Excellent |
| Multi-Language | ✅ | ✅ | ❌ TS only | ✅✅ Wide support |
| Frontend Flexibility | ⚠️ | ✅✅ Excellent | ✅ | ❌ |
| Public APIs | ✅✅ Great fit | ⚠️ Risk | ❌ Internal | ❌ Not ideal |
| Microservices | ⚠️ | ❌ | ❌ | ✅✅ Designed for it |
| Mobile Clients | ⚠️ | ✅ | ✅ | ✅✅ Efficient |
| Best Fit For | CRUD APIs | Complex UI Apps | Fullstack TS Apps | Microservices |

### Legend
- ✅✅ = Excellent/Optimal
- ✅ = Good/Supported
- ⚠️ = Possible with trade-offs
- ❌ = Not recommended/supported

## 🧠 Final Thoughts

Choosing the right API architecture depends on your project's needs, team setup, and scale:

- Use **REST** for simple, public APIs with broad client support
- Use **GraphQL** when your frontend needs fine-grained data control
- Use **tRPC** when you want developer speed and type safety in TypeScript projects
- Use **gRPC** when building scalable microservices or performance-critical applications

Each approach has trade-offs, so understanding the context of your application will help you pick the right tool.
