# REST API

### What is an API?
An <b><i>API(Application Programming interface)</i></b> is a set of rules and protocols that allows different software applications to communicate with each other. An API defines the methods and data formats that applications can use to request and exchange information.

<br><br>
## What is a REST API?
<b><i>REST(Representational State Transfer)</i></b> is an architectural style i.e. an API that adheres to the REST principles. It allows for communication between a client and a server over HTTP protocol, using HTTPS methods(such as GET, POST, OUT, DELETE) to perform CRUD operations on a resource.
- The key abstraction is a <b><i>resource</i></b>. Everything in REST is treated as a resource.
- Resources are typically identified by <b><i>URIs (Uniform Resource Identifiers)</b></i>, such as `/users/123` for accessing a user resource with ID 123.
- Resources are represented in different formats, with JSON being the most common format for RESTful APIs
<div align="center">
  <img src="https://github.com/vipuldhurve/system-design/blob/main/assets/rest-api.jpg" alt="Image" style="display:block; margin:auto;">
</div>

### HTTP Methods:
1. **GET:** Retrieve a resource or a collection of resources.
2. **POST:** Create a new resource.
3. **PUT:** Update an existing resource or create a resource if it doesn't exist.
4. **DELETE:** Remove a resource.
5. **PATCH:** Partially update an existing resource.
6. **HEAD:** Retrieve the headers of a resource, similar to a GET request, but without the response body.
7. **OPTIONS:** Describe the communication options for the target resource.

**NOTE:** GET, HEAD, PUT, OPTIONS and DELETE methods are <b><i>idempotent</i></b>, meaning repeated requests with the same data will result in the same outcome.

### HATEOAS Support 
REST APIs can implement HATEOAS(Hypermedia as the Engine of Application state) to guide clients through the available actions dynamically via hypermedia links sent in responses thus allowing navigation, making the API self-documenting and discoverable.

### Securing a RESTful API
Common security practices include
- Using HTTPS to encrypt data in transit.
- Implementing authentication and authorization mechanisms(e.g. OAuth, JWT, API keys).
- Validating and sanitizing inputs to prevent attacks like SQL injection.
- Using API gateways and rate limiting to control and monitor access.
- Properly handling CORS(Cross-Origin Resource Sharing) configurations.

### Versioning
Versioning in RESTful APIs is crucial for maintaining backward compatibility while allowing the API to evolve and introduce new features. There are several strategies to implement versioning, each with its pros and cons. Here are the most common approaches:
- <b><i>URI Versioning:</i></b> The version number is included in the URI path. This is the most straightforward and widely used method. Example `api.example.com/v1/orders`.
- <b><i>Query Parameter Versioning:</i></b> The version is specified as a query parameter in the request URL. Example: `api.example.com/orders?version=1`.
- <b><i>Header Versioning:</i></b> The version is passed in the Accept header, usually by defining a custom MIME type. Example of Request Header -> `Accept: application/vnd.example.v1+json`.
- <b><i>Custom Header Versioning:</i></b> A custom header is used to pass the version number. Example of Custom Header -> `API-Version: v1`.

#### Common HTTPS Status codes
- `200 OK`: The request was successful.
- `201 Created`: The resource was successfully created.
- `400 Bad Request`: The request was invalid or cannot be processed.
- `401 Unauthorized`: Authentication is required or has failed.
- `403 Forbidden`: The server understood the request but refuses to authorize it.
- `404 Not Found`: The requested resource was not found.
- `500 Internal Server Error`: A generic server error occurred.
- `503 Service not available`: Indicates that the server is currently unable to handle the request.

#### Best practices
- Use nouns in resource URIs, not verbs (e.g., /orders instead of /getOrders).
- Maintain consistent resource naming conventions (e.g., snake_case or camelCase).
- Use proper HTTP methods and status codes.
- Implement versioning for your APIs (e.g., /v1/orders).
- Document your API clearly for users.

<br><br>
## Principles of REST API
<div align="center">
  <img src="https://github.com/vipuldhurve/system-design/blob/main/assets/rest-api-principles.jpg" alt="Image" style="display:block; margin:auto;">
</div>

### 1. Client Server Decoupling
- REST separates the user interface (client) from the data storage (server). Generally, the client handles the user experience, and the server handles the data processing and logic.
- In a REST API design, client and server programs must be independent. 
- The client software should only know the URI of the requested resource. It should have no additional interaction with the server application.

### 2. Uniform Interface
- RESTful APIs use a consistent and standard interface to interact with resources, making the API easy to understand and use. 
- The uniform interface is typically based on standard HTTP methods like GET, POST, PUT, DELETE, etc.
- All API requests for the same resource should look the same regardless of where they come from. 

### 3. Statelessness
- Each request from a client to the server must contain all the necessary information needed to understand and process the request. 
- The server does not store any state about the client session.

### 4. Cacheable
- Wherever feasible, resources should be cacheable on the client or server side.
- Server responses must additionally indicate if they are cacheable or not cacheable.
- RESTful APIs can control caching behavior using HTTP headers like `Cache-Control`.

### 5. Layered System
- REST allows for a layered architecture where intermediaries like load balancers, caches, or proxies can be introduced between the client and server without affecting the communication between them.
- REST API requests and responses are routed through many layers or tiers.
- REST APIs must be designed so neither the client nor the server can tell whether they communicate with the final application or an intermediary.

### 6. Code on Demand (Optional)
- REST allows the server to send executable code to the client in response to requests.
- This is less commonly implemented in RESTful APis.

<br><br>
## Features of REST API

### Scalability
- <b><i>Horizontal scaling:</i></b> REST APIs are stateless, meaning each request is independent and contains all the necessary information. This makes it easier to scale services horizontally by adding more servers to handle requests.
- <b><i>Load Balancing:</i></b> Since requests are stateless, they can be distributed across multiple servers without session affinity, improving load distribution and reliability.

### Flexibility and Portability
- <b><i>Language and Platform Agnostic:</i></b> REST APIs use standard HTTP protocols,making them accessible from any programming language or platform that can make HTTP requests. This allows developers to build clients in various languages and environments.
- <b><i>Interoperability:</i></b> REST APIs can be consumed by a wide range of clients, including web browsers, mobile apps, IoT devices, and other services, fostering interoperability between different systems.

### Maintainability
- <b><i>Versioning:</i></b> REST APIs can be versioned easily, allowing developers to introduce new features or changes without breaking existing clients.

### Performance
- <b><i>Caching:</i></b> REST API supports caching of responses, reducing server load and improving response times. Clients can cache response based on HTTP headers like `Cache-Control`, reducing the need to repeatedly request same data.

### Security
- <b><i>HTTPS Support:</i></b> REST APIs can be easily secured using HTTPS to encrypt data in transit, protecting it from interception and tampering.
- <b><i>Authentication and Authorization:</i></b> REST APIs can implement various authentication and authorization mechanisms, such as OAuth, JWT(JSON Web Tokens), and API keys, to control access to resources.