# Communication between Systems - REST, GRPC, GraphQL, and Service Discovery

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

- Status ACCEPTED: Return it when there is an assyncronous treatment, and create other endpoint to know about the its status (ex: the progress)
- Status NO_CONTENT: Return it when there is no payload and there is a success return
-     
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
Table For Application Exception:

```

|   ID  |   HTTP Status Code           |   Cenário(s)                           |   Mensagem
|-------|------------------------------|----------------------------------------|-----------------------------------------------------------------
|   1   |   500 Internal Server Error  |   Erros não mapeado (ex: nullpointer)  |   We could not handle your request, please try again later.
|   2   |   503 Service Unavailable    |   Indisponibilidade da plataforma      |   Service unavailable. We are currently working to restore it.

```

Table for Business Exception:

```
|   ID   |   HTTP Status Code  |   Cenário(s)                                           |   Mensagem                                                                                                        
|--------|---------------------|--------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------
|   101  |   400 bad request   |   Atributo obrigatório                                 |   Mandatory field '{0}' not informed.                                                                             
|   102  |   400 bad request   |   Data inválida                                        |   Start date entered in the field '{0}' greater than current date.                                                
|   103  |   400 bad request   |   Data inválida                                        |   Error on validate date range, 'from' parameter date is later than 'to' parameter date, should be the opposite.  
|   104  |   400 bad request   |   Data inválida                                        |   Invalid date format in field '{0}'. Format accepted: yyyy-MM-dd                                                 
|   105  |   400 bad request   |   Data inválida                                        |   Invalid date entered in the field '{0}'.                                                                        
|   106  |   400 bad request   |   Atributo obrigatório                                 |   When informing a search range, both fields 'from' and 'to' are mandatory.                                       
|   107  |   400 bad request   |   String mal formada                                   |   Field: '{0}' is a string and must be entered between quotation marks.                                           
|   108  |   404 Not found     |   ExternalIdentifier não existe                        |   Data not found. '{0}'                                                                                           
|   109  |   404 Not found     |   Valor máximo excedido                                |   Maximum value for '{0}' should be lower than or equal to '{1}'.                                                 
|   110  |   404 Not found     |   Valor mínimo excedido                                |   Minimum value for '{0}' should be greater than or equal to '{1}'.                                               
|   111  |   400 bad request   |   Tipo de dado inválido                                |   Field: '{0}' should be an integer.                                                                              
|   112  |   400 bad request   |   Tipo de argumento inválido                           |   Argument type invalid '{0}'.                                                                                    
|   113  |   400 bad request   |   externalIdentifier ou institutionId não informado    |   Must inform at least one of the following: externalIdentifier or institutionId                                  
|   114  |   400 bad request   |   Valores de paginação fora do limite                  |   Pagination values too large.                                                                                    
|   115  |   400 bad request   |   Valores de paginação com caracteres sem ser números  |   Incorrect input paging value, try using numbers only.                                                           
|   116  |   400 bad request   |   Dado já existente                                    |   Data already exists.                                                                                            
|   117  |   400 bad request   |   connectionId ou externalIdentifier não informado     |   Must inform at least one of the following: connectionId or externalIdentifier.                                  
|   401  |   401 unauthorized  |   Token Inválido ou expirado                           |   Unauthorized.                                                                                                   

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
- Communication Format:
    1) Unary: Request -> Response
    2) Server Streaming: Request -> Response (however, you can receive a lot of responses at the same time. Ex: percentage of a calc)
    3) Client Streaming: Request -> Response (however, you can send a lot of requests and receive one response)
    4) Bi Directional Streaming: Request -> Response (however, you can send a lot of requests and receive a lot of responses)
- Difference between REST and gRPC:
    - Rest: Text/Json, Unidirectional, High Latency, No Contract (higher error chance), no streaming support, pre-defined design (post, put, get, delete).
    - gRPC: Protocol Buffers, Bidirectional and Async, Low Latency, Defined Contract, Streaming Support, free design, code generation.
- Difference between gRPC and Proto Buffers (Together you can work with gRPC)
    - Protobuff: https://protobuf.dev/
    - gRPC: https://grpc.io/about/

## Service Discovery
- Which machine should we call? Which port to use? Do we need to know the instance IP? How to be sure that the instance is healthy? How to know that we have permission to access?
- These are some questions that the service discovery can answer. When we have more than one service identically, what should you do?
- It figures out the machines available to access (the health ones)
- Segmentation of machines to guarantee security (how machines will access others, and what rules some machines should have to access or not other)
- DNS resolution
- Active Health Check (time to time is checked)
- Tool: Consul (By Hashicorp)
    - Agnostic to programming language
    - Load Balancing (Layer 7)
    - Service Segmentation
    - Key/Value Configuration (Secrets, env variables, etc)
    - Security: Mutual TLS, Service Mesh, etc.
    - Service Registry: the place where we have all the services registered.
    - The health check is done locally. The consul that is running in the app send this information to the Consul Server
    - The Consul has its own DNS Server
    - Active Health Check: The consul has an Agent in each service. It informs the Consul Server its availability (The consul server does not verify the server, it is the opposite: the agent warns the consul server)
    - Multi Cloud: You can create a communication among Clouds through Consul
    - Agent: A process that is executed in every cluster node. It can be Client mode or Server Mode.
    - Client: Register the service locally, health check and forward the information and configuration to the Server
    - Server: Commonly, there are more than one server and there is one server leader. Features: keep the cluster state, register the services that were informed by the clients, membership (who is the client and who is the server), return of queries (DNS or API), exchange information between datacenter, etc.
    - Dev Mode Feature: simulate a server in consul (just to test, not for production, run as a server, do not scale, register everything in memory)
    - OBS: You have to set up an Agent (Client) to each service that you will deploy (to communicate with the servers).
    - OBS: you can communicate to the server through the client (requests to the client you can get information from the server, because everything is synchronized)
    - parameter "encrypt": You have to execute "consul keygen" and put the value in all the "servers.json". This way all the communication will be encrypted. All the servers have to have the encrypt to join with each other. Also, in production you have to work with TLS, too.
    - Interface to manage all the consul resources: access the "consulserver01:8500"
    - OBS: Kubernetes has its own Service Discovery 
