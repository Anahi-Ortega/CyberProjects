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

### Discovering API Documentation   
API doc isn't always openly available, you may still be able to access by browsing applicatoins that use the API. 

Burp Scanner can crawl the API. You can also browse manually using Burp's browser. Endpoints that may refer to API docs may include: 

- `/api`
- `/swagger/index.html`
- `/openapi.json`

If you identify an endpoint for a resource, make sure to investigate the base path. For example, if you identify the resource endpoint `/api/swagger/v1/users/123`, then you should investigate the following paths:

- `/api/swagger/v1`
- `/api/swagger`
- `/api`

You can also use a list of common paths to find documentation using Intruder.

### Using machine-readable documentation 
A range of automated tools can analyze any machine-readable API documentation you find. 

You can use Burp Scanner to crawl and audit any docs in JSOn or YAML format. You can also use OpenAPI Parser to parse the documentation. 

Postman or SoapUI are also credited as specialized tools to test the documentation enpoints. 

## Identifying API endpoints
You can also browse the applications that use the API and gather information on the API use. This method can be used to challenge inaccurate or out-of-date documentation. 

Burp Scanner offers the ability to crawl the applicatoin then allows you to manually investigate the attack surfave with the Burp browser. 

While browsing the application, you may find patterns that suggest API endpoints in the URL structure. You can also keep an eye out for JavaScript files which may contain refereces to API endpoints. Burp Scanner automatically extracts some endpoints during crawls, but for more heavyweight extraction, use JS Link Finder. 

### Interacting with API endpoints
Once you have identified API endpoints, you can use Burp Repeater and Burp Intruder to interact with them, observe the APIs behavior, and discover additional attack surfaces.

Review error messages and other responses closely, sometimes these include information you can use to contruct a valid HTTP request. 
#### Identifying supported HTTP methods
The Hyper Text Transfer Protocol specifies the action to be performed on a resource. Examples include: 
`GET` - Rertreives data from a resouce  
`PATCH` -  Applies partial changes to a resource
`OPTIONS` -Retreives information on the types of request methods that can be used on a resource

An endpoint may support different HTTP methods so test all potential methods when as you discover all of the endpoints. 

You can use the built-in HTTP verbs list in Burp Intruder to automatically cycle through a range of methods. 

>{!NOTE} IMPORTANT: When testing different HTTP methods, target low-priority objects. This helps make sure that you avoid unintended consequences, for example altering critical items or creating excessive records.

### Identifying supported content types
API endpoints often expect data in a specific format and may behave differently depending on the content type of the data in the request. Changing content may emable you to: 
- Trigger errors that disclose useful information
- Bypass flawed defenses
- Take advantage of differences in processing logic. 

Example: An API may be secure when handling JSON data but susceptible to injection attacks when dealing with XML.

To change the content type modify the `Content-Type` header, then reformat the request body accordingly. You can user the Content type converter BApp to automatically convvert data submitted within requests between XML and JSON. 