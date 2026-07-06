---
title: "HTTP Methods Overview"
date: "2015-06-09T16:48:17"
draft: false
comments: false
socialShare: true
toc: false
tags: 
  - web dev
  - http
---

HTTP methods, also known as HTTP verbs, are fundamental to web development as they define the action to be performed on a given resource identified by a Request-URI. They are essential in RESTful APIs, where the method signifies the desired action on the server's data.  

Understanding these methods is crucial for web developers as they directly correspond to the basic operations in any data-driven application, often described as CRUD (Create, Read, Update, Delete). Here's an overview of the most common HTTP methods and their usage in the context of a simple web API, illustrating their roles in interacting with a Car Dealership database.  

The scenario: 

Imagine you're tasked to create a simple RESTful API that hits a Car Dealership database with only two entities, Make and Model. It's a one-to-many relationship where a particular Make can have many Models. For example, Toyota (Make) produces the Camry, Tundra, 4Runner, etc (Models).

```json
General uri format: 
api/makes/{makeId}/models/{modelId}
```

### Reading Resources

```json
Request:
GET api/makes

Response:
200 Ok -> [{make}, {make}, ...]
404 Not Found
```

We send a GET request if we want a list of resources. A successful response will return 200 Ok and said resource list in the response body. We return 404 Not Found if the uri doesn't exist.

```json
Request:
GET api/makes/{makeId}

Response:
200 Ok -> {make}
404 Not Found
```

If we want to read a resource, we add the id. A success will return that resource in the response body. A 404 is returned if the resource doesn't exist.

### Deleting Resources

```json
Request:
DELETE api/makes/{makeId}

Response:
204 No Content 
404 Not Found
```

We return a 204 No Content after a successful DELETE request of a resource and return a 404 if the resource is not found. No Content means the server has successfully fulfilled the request and that there's nothing to be sent with the response body.

```json
Request:
DELETE api/makes

Response:
204 No Content 
404 Not Found
```

Deleting a list of resources follows the same guidelines as above but it's rarely done as it can be quite destructive. Generally, mass deletion is not allowed.

### Creating Resources

When creating resources, we have to differentiate between requests where the responsibility of creating the URI is at the server or at our API consumer. 

#### Server

```json
Request:
POST api/makes <- {make}

Response:
201 Created -> {make}
404 Not Found
```

To create a new car Make, we send a POST request to the Makes resource with the Make object as the request body. If creation is successful then the status code is 201 Created and the response contains the created Make object in the response body and the location in the location header. If the Makes resource doesn't exist then we return 404 as usual.

```json
Request:
POST api/makes/{makeId}

Response:
404 Not Found
409 Conflict
```

Issuing a POST request to a single resource URI can never be successful because either a 404 Not Found is returned if the Make doesn't exist or a 409 Conflict is returned because that particular Make already exists. 

```json
Request:
POST api/makelist <- {makeList}

Response:
201 Created -> {makeList}
404 Not Found
```  

Adding a list of resources, if allowed, should have a new resource URI created for that list. For example, we can call it MakeList and a POST request to that resource will contain an array of Makes. We return a 201 Created with the new list in the response body for a successful response and return 404 if that resource list doesn't exist.

#### Consumer

```json
Request:
PUT api/makes/{makeId} <- {make}

Response:
201 Created -> {make}
``` 

When the consumer of our api is allowed to create resource uris, we can ["upsert"][Upsert] to create resources. For example, a PUT request is issued to update an existing resource, fine, but if the same PUT request is issued on a resource that doesn't exist then that will result in the resource being created. Hence, why a 201 Created is returned with the newly created Make in the response body. A 404 Not Found will never occur because a non-existent resource will be created. 

```json
Request:
PATCH api/makes/{makeId} <- {JsonPatchDocument on make}

Response:
201 Created -> {make}
``` 

Upserting with a partial resource update with a PATCH request works identically to upserting with PUT except that a json patch document is included in the request.

### Updating Resources

```json
Request:
PUT api/makes/{makeId} <- {make}

Response:
200 Ok -> {make}
204 No Content
404 Not Found
```  

Sending a PUT request on a resource updates that resource. Since it's a PUT, we should pass in the request the full Make object or else the omitted fields will be set to their default values. We return 200 Ok with a successfully updated resource in the response body or just state 204 No Content. Non-existent resources return a 404 Not Found, unless we're [upserting][Upsert]. 

```json
Request:
PUT api/makes <- [{make}, {make}, ...]

Response:
200 Ok -> [{make}, {make}, ...]
204 No Content
404 Not Found
```

Using a PUT request for mass updates is typically not allowed but can be done. It follows the same guidelines as updating a single resource except the resource list is passed in the request and the updated list is returned in the successful response body. Non-existent resources return a 404 Not Found, unless we're [upserting][Upsert]. 

```json
Request:
PATCH api/makes/{makeId} <- {JsonPatchDocument on make}

Response:
200 Ok -> {make}
204 No Content
404 Not Found
```

Partial updates with a PATCH request is similar to a PUT request. Except we don't have to pass a full resource object in the request. We list the fields we want to update in a json patch document and the omitted fields will keep their existing values. A successful response can return 200 Ok with the patched resource object in the body or simply show a 204 No Content. Non-existent resources return a 404 Not Found, unless we're [upserting][Upsert]. 

```json
Request:
PATCH api/makes <- {JsonPatchDocument on make}

Response:
200 Ok -> [{make}, {make}, ...]
204 No Content
404 Not Found
```

Using PATCH to mass update resources, like most mass operations, is rarely allowed. If it's allowed then it follows the same guidelines as PATCHing a single resource. 

[Upsert]: https://en.wikipedia.org/wiki/Merge_(SQL) 
