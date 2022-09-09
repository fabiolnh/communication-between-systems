# communication-between-systems
Communication Between Systems (REST, GRPC, GraphQL, Service Discovery, Consul)

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
