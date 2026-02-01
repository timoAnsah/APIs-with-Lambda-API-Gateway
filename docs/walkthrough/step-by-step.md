# Step-by-step Implementation Guide

In this project I will demonstrate how to set up an API using AWS Lambda & Amazon API Gateway 
I am doing this to learn how APIs work & also set up the logic tier for this three tier architecture.

## Step 1 - The Lambda Function 
- Create a Lambda Function
- Condfigure the settings
- add code to retrieve user data

Lambda function will be the brain of my application, it's a service that lets me run a code without having to manage servers. its a Function as a Service. In this use case it will fetch data from a database & return it to users. A common use case in Web Applications.
You can write Lmabda Functions to do things like process data, respond to events & run automated tasks.
for example when you search for a product in an online shop, the website backend needs to query the product database,
find the relevant product & send the details back to the browser as quickly as possible.

Lambda runs the code only when needed which (no payment for idle time)
It scales automatically, from a few requests per day to thousans per second. The only requirement is to supply a code in a language supported by Lambda, in this case I'lll be using python.

<img width="1919" height="247" alt="lambda step 1" src="https://github.com/user-attachments/assets/45950f00-822c-4672-b244-46e122d8b09d" />

<img width="1233" height="821" alt="the function" src="https://github.com/user-attachments/assets/22cac507-69d1-4b95-b6ef-882fa2ec8f53" />


- Create Lambda function & Select Author from scratch
- functions name `Retrieve Data`
- Run time is the enviroment used to run the code. Python code needs a Python runtime environment to execute our Lambda Function.
- execution role defines permissions of the function. sometimes Lambda needs permissions to access other service.
We create basic lamdbda permissions to write logs to CloudWatch to troubleshoot errors. 
- For Architecture I selected x86_64, it is the physical processor/hardware somewhere in the world of AWS which runs the code.

<img width="1205" height="444" alt="lambda" src="https://github.com/user-attachments/assets/f77d607c-5387-41f6-993c-174dbbb3f550" />

Successfully created the Lambda Function.

# Function & Code
The next step is to create a Function & give it a purpose. I added a code into the function editor.

<img width="1272" height="1010" alt="dynamo code" src="https://github.com/user-attachments/assets/691edf26-cbb8-451d-a569-bda202a90a69" />



This code sets up a Lambda Function that retrieves data from a DynamoDB table.
It grabs `UserID` from a triggered event i.e. submitting a UserID through a field on a website & queries for a piece of data in Dynamo DB that matches the `USERID`.
If there is an error, `UserID` does not exist in the database & an error message is returned.

Next I deployed the code which makes the function ready to use.
With the Lambda Function ready I need a way to access it & this is where AP
I Gateway comes in.

## Step 2 - Set Up an API Gateway

Application Programming Interface, its a protocol software systems use to communicate with each other.
API Gateway makes it easy to create, publish, maintain, monitor & secure API at any scale.
It manages incoming traffic, directs them to the correct service & makes sure only authorised request gets through.
Its like a messenger that carries requests & responses between systems. 
Specifically for this project, we're creating an API that will be the runner between the Users Request in the browser & the Lambda Function that will process these request.API also returns the Lambda Functions response.
Its great as a security checkpoint it makes sure only authorised request come through, it can receive events coming in & determine if they are authorised to do it and send the request to a Lambda function
which then returns the request with an authorised or unathorised access.

## Relationship between APIs & Lambda
- Front door to Lambda Function - Receives request & forwards them to Lambda for processing
- Lambda processes the request & sends the response through API gateway back to user.

Worth noting that exposing Lambda Functions directly to User request is not best practice. 
Lambda Functions do not have built in security or API management features. 
API Gateway brings in authentication, authorization features & advanced API management capabilities like request routing that makes your app more efficient.

<img width="2562" height="1482" alt="API" src="https://github.com/user-attachments/assets/aa657dd6-b0f6-4923-9524-8bf988dada3f" />


API Gateway supports different typres of API, examples are REST, WebSocket & HTTP.
They are suited for different use cases.
- HTTP - Routing request
- WebSocket - Real Time capabilities
- REST - Maintaining a standard web API

For this project I am using a REST API. 
Representational State Transfer is a type of API that uses HTTP methods to interact with resources.
it's simple & can be used with any programming language & connect User with the Lambda Function.

<img width="2737" height="502" alt="REST" src="https://github.com/user-attachments/assets/573fdb32-1c61-49d9-8661-004dcbcf7d69" />

<img width="2750" height="1417" alt="CreateREST" src="https://github.com/user-attachments/assets/d256e51a-7b13-4c9b-b932-6c6adba4dce9" />

When configuring the API
- Selected Build
- Under API details, select New API
- API name is `UserRequestAPI`
- API endpoint type I selected `Regional`

Endpoint types define the scope of the API's availability. 
Regional endpoints are accessible within a specific AWS region, which is great for localized applications because it has low latency for clients in that region.

Other endpoint types are Regional, there are Edge-Optimized (for global applications) and Private (for internal networks) endpoints.

Each serves different needs depending on how and where you want your API accessed.

<img width="2710" height="885" alt="RESTAPI" src="https://github.com/user-attachments/assets/47fc1039-6810-446f-926f-df0612efde80" />

This is the central hub for managing & monitoring APIs methods, deployments & settings

## Set Up API Resource

The next step was to create an API Resource to help me organise what I want it to do.

Resources are individual endpoints within an API that handles different parts of it's functionality & organise how APIs handles different requests.
For example I might have a resource for users, products & orders. Each resource has its own set of actions that can be performed on it,
like creating, reading, updatinng or deleting data.


- Under resource, select create resource

<img width="1311" height="363" alt="APIresource" src="https://github.com/user-attachments/assets/dcc9742c-2225-44aa-8b09-60688beca78d" />

Resource path & Resource name is the URL path used to access that resource e.g. `/Messages` for retrieving messages & `/Users` for retrieving User profiles.
The resource name is used in the API Gateway to refer to that resource.

<img width="1306" height="348" alt="APIresource" src="https://github.com/user-attachments/assets/fd98025b-161b-41e9-843a-d225a5279b5a" />

<img width="1311" height="258" alt="theresource" src="https://github.com/user-attachments/assets/30e981db-cfb8-4aac-bcaa-2ab1e6e948fb" />

since the resource can be used for many different kinds of actions, we'll need to define what each of those actions are.

## API Method

Methods are things I can do with the resource.
For example:
- `GET` method to retrieve data
- `POST` method to create data
- `PUT` method to update data
- `DELETE` method to delete data


- Create a GET Method for the `Users` resource
- Connect the `GET` method to the Lambda Function
- Configure the method


The next I will create a method in `/Users` resource. This is because I can perform different actions in the same resource.
Methods define which actions are possible & how they can be carried out

<img width="1311" height="335" alt="createMETHOD" src="https://github.com/user-attachments/assets/714f9b6a-f3e9-4553-94e9-e59dd3553dd7" />

In this next step the `Method` type is `GET` to retrieve data.
The `GET` method will also connect the API Gateway with my Lambda Function.
When an end user makes a request with my API e.g. "api.com/users", API knows to pass the event to my Lambda Function when i'm retrieving data.
the integration type is a Lambda fucntion. 

An integration type is a backend service tat can fulfill an API request. It determines how API Gateway passes the request data to the backend & processes the response.
By selecting the Lambda Function as the integrtion type, API Gateway can directly call the AWS Lambda fucntion each time an API endpoint is used:
- When API gets a request, it sends it directly to my Lambda Funtion
- The Lambda Function takes care of the request, processes it & sends the result back to API Gateway
- API Gateway passes the response on to the Users.

- <img width="1299" height="356" alt="GETmethod" src="https://github.com/user-attachments/assets/04f49d79-98dc-40ca-9c3a-eb4257b2fe78" />

- Switch on Lambda Proxy Integration
- Select `RetrieveUserData` function
- Select create Method
- Ensure the default region where the Lambda Function is created 

Lambda Proxy Integration is a setting that simplifies the connection between API Gateway & Lambda Funtion
When users interact with a website, a request for data from the database goes straigh to the API Gateway. 
This request contains multiple parts like headers, query & path parameters & more
The API Gateway breaks down the request & formats it in a way the Lambda Funtion can process. 
Lambda returns a response, API Gateway needs to map the response back into a format expected by the User.

With Lambda proxy integration, API Gateway doesn't need to reformat the user's request. 
Instead, it passes the entire request - headers, query parameters, path parameters, and body directly to Lambda. 
The Lambda function itself will have to be capable of processing the request internally

<img width="1301" height="1010" alt="GETmethoddone" src="https://github.com/user-attachments/assets/4c6dba36-9bc1-43a6-a26f-c2364870f943" />

The next step 
