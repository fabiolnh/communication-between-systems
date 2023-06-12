# Communication between Systems - REST, GRPC, GraphQL, Service Discovery, Consul (Currently Studying)

## Synchronous Communication: 
- Real Time
- Send a request and receive a response in the right time

## Asynchronous Communication:
- You do not need the answer in the right time
- Commonly we can work this way with microservices
- You send the message to a separated system and this system will be responsible for the delivery (message broker: Kafka, RabbitMQ, SQS)
- You do not depend on microservices

## Rest
- Stateless (the server does not keep state)
- Cacheable Resources
- Maturity:
    - Level 0 Maturity: No Pattern
    - Level 1 Maturity:
        - GET /products/1
        - POST /products
        - PUT /products/1
        - DELETE /products/1
    - Level 2 Maturity (Always use for):
        - GET /products/1 (for recover information)
        - POST /products (for insert)
        - PUT /products/1 (for alter)
        - DELETE /products/1 (for delete) 
    - Level 3 HATEOAS (Hypermedia as the Engine of Application State) (the best one)
        - It responds on the payload of the endpoint with what you can do later with those information
        - Ex: next pages, other endpoints, etc.

- Use unique URIs
- Use all the http verbs, including cache
- Provide relational links exemplifying the resources of what could be done with it

- Method Negotiation:
    - OPTIONS: check what methods (verbs) can be used with that resource
- Content Negotiation: (how the client wants the answer from the server)
    - Accept (header): application/json, or xml, etc.
    - You can work with versioning using: "application/vnd.project.v1+json". This way you do not need to create "/v1, /v2" in each uri of the endpoint
- Content Type Negotiation: (how the client will send the payload to the server)
    - Content-type: application/json, or xml, etc.

- Interesting: Install on VSCode the extension: "Dev Containers", then install the container "PHP", then:
    - https://api-tools.getlaminas.org/download
    - inside the terminal, get the laminas
    - composer create-project laminas-api-tools/api-tools-skeleton
    - apt-get update
    - apt-get install sqlite3
    - cd api-tools-skeleton
    - touch test.sqlite
    - sqlite3 test.sqlite
    - create table users(id int, name varchar(255), email varchar(255))
    - .tables
    - control C
    - php -S 0.0.0.0:8080 -t public public/index.php
    * With this tool you can check payload returns, status, etc, with a lot of conventions.


## Extra

Payload Rest Example (not from conventioning):

Success with list:
```
{
    "data": [
        {
            "attribute": "...",
        }
    ]
    "pagination": {
        "totalItems": 16,
        "totalPages": 1,
        "size": 100
    }
}
```
Success without list:
```

{
    "data": {
        "attribute": "...",
    }
}
```

Error:
```
{
    "errors": [
        {
            "code": "XXX",
            "message": "Message"
        }
    ]
}
```

## GraphQL
- Good for queries from front-enders
- However, only a few people use it
- It is a RPC with a default procedure providing a query language
- A call in a format that the server can understand that requires only the fields that you want
- Good for BFF (Backend for frontend) 
- Created by Facebook
- for go language:  https://gqlgen.com/

## gRPC
- It is a framework created by Google, but maintained by CNCF (Cloud Native Computing Foundation)
- More secure and Fast than Rest
- Language Independent
- Good for microservices
- There is a web project, but it is not mature enough to use in frontend
- In some languages, you can generate the code for GRPC
- Bidirectional Streaming using HTTP/2 (it creates a channel to communicate)
- Protocol Buffer (Protobuf): 
    - A binary file (smaller than a json)
    - The serialization is faster (it uses a binary file, different than a json)
    - Less traffic resources
    - Faster process (http2 and smaller file)
- Profo File (Protofile):
    - A file declaring the contract between the communication
    - It is a .proto
 - HTTP/2:
    - Launched in 2015
    - The data traffic is binary (HTTP 1.1 is text)
    - Use the same TCP connection to receive or send the data (Multiplex): Open the connection, send and receive whatever you want, and then you close the connection
    - It is good to check if your application is using Http2, because it is very good
    - Server Push (In the server, you can send information to the frontend)
    - Headers as compressed
    - Less network resources
    - Faster
