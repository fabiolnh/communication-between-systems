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
- Level 3 HATEOAS (Hypermedia as the Engine of Application State)
    - It responds on the payload of the endpoint with what you can do later with those information
    - Ex: next pages, other endpoints, etc.


## Extra

Payload Rest Example:

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
