# API Service

## Introduction
The ASAB library is a lightweight, asynchronous framework for building scalable and high-performance applications in Python. It provides various features for building microservices, such as a message bus, a scheduler, and an API service. This documentation focuses on the API service module of ASAB.

## ASAB API Service
The ASAB API service is a module that provides a web interface for accessing and interacting with other microservices in an ASAB-based application. It allows developers to easily create HTTP endpoints that can be accessed by clients over the internet.

### Installation
To use the ASAB API service module, you must first install the ASAB library. You can install it using pip:

```
$ pip install asab
```

### Usage
The API service module provides a Service class that you can use to create HTTP endpoints. Here is an example of how to create a simple HTTP endpoint that returns a JSON object:

```
from asab.api.service import Service

async def hello(request):
    return {"message": "Hello, World!"}

if __name__ == "__main__":
    app = Service()
    app.router.add_get("/", hello)
    app.run()
```
In this example, we define a simple function called "hello" that returns a JSON object with a "message" key. We then create a new instance of the Service class and add our "hello" function to its router using the add_get() method. Finally, we start the web server using the run() method.

### HTTP Methods

The Service class supports the following HTTP methods:

- GET
- POST
- PUT
- DELETE

You can add endpoints for each of these methods using the following methods of the router object:

- add_get()
- add_post()
- add_put()
- add_delete()

For example, here is how you can create an endpoint that accepts a POST request and returns a JSON object:

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

### URL Parameters

You can also define endpoints that accept URL parameters. To do this, you can use the curly brace syntax to define a parameter in the URL pattern, and then access the parameter value in your function using the request.match_info object.

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

### Authentication

The ASAB API service module provides several authentication mechanisms that you can use to secure your endpoints. You can use the authentication middleware provided by the aiohttp library or implement your own custom middleware.

To use the aiohttp authentication middleware, you can use the app.middlewares.append() method to add it to the list of middlewares used by the application:
