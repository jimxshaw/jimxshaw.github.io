---
layout: post
title:  "HTTP Method Summary"
date:   2017-06-09 16:48:17 -0500
categories: web-development
---

Here's a summary of the most common HTTP methods by use case.

The scenario: 

Imagine you're tasked to create a simple web api that hits a Car Dealership database with only two entities, Make and Model. It's a one-to-many relationship where a particular Make can have many Models. For example, Toyota produces the Camry, Tundra, 4Runner etc.

```
General uri format: 
api/{makes}/{makeId}/{models}/{modelId}
```

### Reading Resources

```
Request:
GET api/makes

Response:
200 Ok -> [{make}, {make}, ...]
404 Not Found
```

We send a GET request if we want a collection of resources. A successful response will return 200 Ok and said resource collection in the response body. We return 404 Not Found if the uri doesn't exist.

```
Request:
GET api/makes/{makeId}

Response:
200 Ok -> {make}
404 Not Found
```

If we want to read a specific resource, we add the id. A success will return that specific resource in the response body. A 404 is returned if the resource doesn't exist.

### Deleting Resources

```
Request:
DELETE api/makes/{makeId}

Response:
204 No Content 
404 Not Found
```

We return a 204 No Content after a successful DELETE request of a particular resource and return a 404 if the resource is not found.

```
Request:
DELETE api/makes/

Response:
204 No Content 
404 Not Found
```

Deleting a collection of resources follows the same guidelines as above but it's rarely done as it can be quite destructive. Generally, mass deletion is barred.

### Creating Resources

When creating resources, we have to differentiate between requests where the responsibility of creating the uri is at the server and where it's at the consumer of our api. 

#### Server
```
Request:
POST api/makes <- {make}

Response:
201 Created -> {make}
404 Not Found
```

To create a new car Make, we send a POST request to the Makes resource with the Make representation as the request body. If creation is successful then the status code is appropriately 201 Created and the response contains the created Make representation in the response body and location in the location header. If the Makes resource doesn't exist then we return 404 as usual.

```
Request:
POST api/makes/{makeId}

Response:
404 Not Found
409 Conflict
```

Issuing a POST request to a single resource uri can never be successful because either a 404 Not Found is returned if the Make doesn't exist or a 409 Conflict is returned because that particular Make already exists. 

```
Request:
POST api/makecollections <- {makeCollection}

Response:
201 Created -> {makeCollection}
404 Not Found
```  

Adding a collection of resources, if allowed, should have a new resource uri created for that collection. For example, we can call it MakeCollections and a POST request to that resource will contain an array of Makes. We return a 201 Created with the new collection in the response body for a successful response and return 404 if that resource collection doesn't exist.

