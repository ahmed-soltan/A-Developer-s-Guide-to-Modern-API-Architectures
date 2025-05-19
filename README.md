# A Developer's Guide to Modern API Architectures

## Introduction
In the ever-evolving world of web development, choosing the right API design can be the difference between a smooth developer experience and a tangled mess of code. From the simplicity of REST to the performance of gRPC, and the type-safety of tRPC to the flexibility of GraphQL — each style brings its own strengths and trade-offs. In this article, we'll break down five popular API architectures — REST, RESTful APIs, GraphQL, tRPC, and gRPC — to help you understand what they are, how they work, and when to use each one.

---

## What is Rest?  

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

### What is HATEOAS?

HATEOAS is a constraint of REST architecture where a server response not only includes the requested data but also hyperlinks (URLs) to related actions or resources. This allows the client to navigate the API dynamically based on the server's responses — without needing to hardcode endpoint paths.

In other words, the server drives the application's state transitions by including links (hypermedia) in the responses.

#### Example: Without HATEOAS
```http
GET /users
[ { "id": 1, "name": "Alice" }]
```
*The client must hardcode the next step: `/users/1`*

#### Example: With HATEOAS
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

**Common Question**: What about authorized requests? Session indicates that we have a state on the server.
> Yes, but each request is still independent of the others, each has session stored in HTTP request headers or cookies so using session does not violate statelessness principle.

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

---

## REST vs RESTful API

### REST (Representational State Transfer)
- Definition: REST is an architectural style for designing networked applications
- REST has 6 principles
- REST is the theory or guidelines

### RESTful
- Definition: RESTful refers to applications, services, or APIs that implement the principles of REST
- Usage: When an API follows the REST architecture (e.g., uses HTTP methods like GET, POST, PUT, DELETE appropriately), it is called a RESTful API
- RESTful is the implementation of REST principles
