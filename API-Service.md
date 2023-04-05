# API Service

## Introduction
Asynchronous Server App Boilerplate (ASAB) is a microservice platform for Python 3.7+ and asyncio that aims to minimize the amount of code that needs to be written when building a microservice or an application server. It provides various features for building microservices, such as a message bus, a scheduler, and an API service. This documentation focuses on the API service module of ASAB.

## ASAB API Service
The ASAB API service is a module that provides a web interface for accessing and interacting with other microservices in an ASAB-based application. It allows developers to easily create HTTP endpoints that can be accessed by clients over the internet.

### Installation
To use the ASAB API service module, you must first install the ASAB library. You can install it using pip:

```
$ pip install asab
```

### Usage

The API service module provides a **Service** class that you can use to create HTTP endpoints. Here is an example of how to create a simple HTTP endpoint that returns a JSON object:

```
from asab.api.service import Service

async def hello(request):
    return {"message": "Hello, World!"}

if __name__ == "__main__":
    app = Service()
    app.router.add_get("/", hello)
    app.run()
```
In this example, we define a simple function called "hello" that returns a JSON object with a "message" key. We then create a new instance of the **Service** class and add our "hello" function to its router using the **add_get()** method. Finally, we start the web server using the **run()** method.

This code allows a developer to easily create a simple HTTP endpoint that returns a JSON object using the ASAB API service module. By customizing the hello function with appropriate logic, developers can generate any type of response they need to serve their particular use case.

### HTTP Methods

The **Service** class supports the following HTTP methods:

- GET
- POST
- PUT
- DELETE

You can use these methods to create HTTP endpoints that can receive requests from clients and respond with appropriate data.

You can add endpoints for each of these methods using the following methods of the router object:

- **add_get()**
- **add_post()**
- **add_put()**
- **add_delete()**

For example, let's say you want to create an endpoint that accepts a POST request and returns a JSON object containing user information. Here's how you can do it using the **add_post()** method:

```
from asab.api.service import Service

async def create_user(request):
    # create a new user and return their information
    user = {"id": 1, "name": "John Doe", "email": "john.doe@example.com"}
    return user

if __name__ == "__main__":
    app = Service()
    app.router.add_post("/users", create_user)
    app.run()
```

In this example, we create a function called **create_user** that generates a JSON object containing user information. We then use the **add_post()** method to add the **create_user** function to the **Service** class router and specify the URL endpoint for the function ("/users" in this case).

### URL Parameters

You can also define endpoints that accept URL parameters. To do this, you can use the curly brace syntax to define a parameter in the URL pattern, and then access the parameter value in your function using the **request.match_info** object.

For example, here is how you can create an endpoint that accepts a user ID parameter in the URL:
The define section includes the following parameters:

```
from asab.api.service import Service

async def get_user(request):
    # get the user ID from the URL parameters
    user_id = request.match_info["user_id"]
    
    # fetch the user information from the database
    user = {"id": user_id, "name": "John Doe", "email": "john.doe@example.com"}
    return user

if __name__ == "__main__":
    app = Service()
    app.router.add_get("/users/{user_id}", get_user)
    app.run()
```

In this example, we create a function called **get_user** that accepts a **request object**. We use the **request.match_info** object to access the user ID parameter from the URL pattern ("/users/{user_id}"). Then, we use the retrieved user ID to fetch the user information from a database or any other source, and return the user information as a JSON object.

### Authentication

The API Service also provides built-in support for authentication. You can use the add_authenticator() method to add an authenticator function to a route:

```
def authenticate(request):
    if 'Authorization' not in request.headers:
        return None
    token = request.headers['Authorization'].split(' ')[1]
    if not verify_token(token):
        return None
    return {'username': get_username(token)}

@api.add_route('GET', '/private')
@api.add_authenticator(authenticate)
async def private(request):
    return {"message": "Welcome, {}!".format(request.user['username'])}
```

In this example, the authenticate() function is called before the private() function. It checks for an "Authorization" header in the request and verifies the token. If the token is valid, it returns a dictionary with user information. If the token is not valid, it returns None and the request is rejected.
