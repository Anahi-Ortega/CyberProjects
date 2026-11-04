## API Testing
**Application Programming Interfaces (APIs) -** enable software systems and applications to communicate and share data. All dynamic websites are composed of APIs, so classic web vulnerabilities like SQL injection could be classed as API testing.   

> [!NOTE]
> This section will have a specific focus on RESTful and JSON APIs.

## API Recon
 Find out as much information about the API as possible, to discover its attack surface.

 To begin, we will first identify the API endpoints (locations where an API receives requests). 

 Example:   
 `GET /api/books HTTP/1.1`  
` Host: example.com`  
Breakdown:  
The endpoint in this request is `/api/books`. In this example the interaction results with the API retreiving a list of books from a library.

Once you have identied the endpoints, you need to determin how to interact with them. This will enable you to construct valid HTTP requests to test the API. Find out the following: 
- The input data the API processes, including both compulsory and optional parameters.
- The types of requests the API accepts, including supported HTTP methods and media formats.
- Rate limits and authentication mechanisms.

## API Documentation
APIs are usually documented so that developers know how to use and integrate with them. Documentation can be in both human-readable and machine-readable forms. 

**Human-readable documentation** is designed for developers to understand how to use the API. It may include detailed explanations, examples, and usage scenarios.  
**Machine-readable documentation** is designed to be processed by software for automating tasks like API integration and validation. It's written in structured formats like JSON or XML.

Docs are often public, always start your recon by reviewing docs. 

## Discovering API Documentation   
API doc isn't always openly available, you may still be able to access by browsing applicatoins that use the API. 

Burp Scanner can crawl the API. You can also browse manually using Burp's browser. Endpoints that may refer to API docs may include: 

- `/api`
- `/swagger/index.html`
- `/openapi.json`

If you identify an endpoint for a resource, make sure to investigate the base path. For example, if you identify the resource endpoint `/api/swagger/v1/users/123`, then you should investigate the following paths:

- `/api/swagger/v1`
-`/api/swagger`
- `/api`

You can also use a list of common paths to find documentation using Intruder.
