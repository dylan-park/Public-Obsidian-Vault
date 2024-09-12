---
tags:
  - devops
  - ccna
---
- ## APIs Overview ^ccna-apis
	- An API (Application Programming Interface) is a software interface that allows two applications to communicate with each other
	- APIs are essential not just for network automation, but for all kinds of applications
	- The NBI typically uses REST APIs
	- NETCONF and RESTCONF are popular southbound APIs
- ## CRUD
	- CRUD (Create, Read, Update, Delete) refers to the operations we perform using REST APIs
	- **Create** operations are used to create new variables and set their initial values
		- i.e. Create variable "ip_address" and set the value to "10.1.1.1"
	- **Read** operations are used to retrieve the value of a variable
		- i.e. What is the value of variable "ip_address"?
	- **Update** operations are used to change the value of a variable
		- i.e. Change the value of variable "ip_address" to "10.2.3.4"
	- **Delete** operations are used to delete variables
		- i.e. Delete variable "ip_address"
	- HTTP uses *verbs* (aka. *methods*) that map to these CRUD operations
	- REST APIs typically use HTTP

| Purpose                      | CRUD Operation | HTTP Verb  |
| ---------------------------- | -------------- | ---------- |
| Create new variable          | Create         | POST       |
| Retrieve value of variable   | Read           | GET        |
| Change the value of variable | Update         | PUT, PATCH |
| Delete variable              | Delete         | DELETE     |

- ## HTTP
	- ### Request
		- When an HTTP client sends a request to an HTTP server, the HTTP header includes information like this:
			- An HTTP Verb (i.e. GET)
			- A URI (Uniform Resource Identifier), indicating the resource it is trying to access
		- The HTTP request can include additional headers which pass additional information to the server
			- An example would be an **Accept** header, which informs the server about the type(s) of data that can be sent back to the client
				- i.e. **Accept: application/json** or **Accept: application/xml**
	- ### Response
		- The server's response will include a status code indicating if the request succeeded or failed, as well as other details
		- The first digit indicates the class of the response:
			- **1xx** *informational* - The request was received, continuing to process
				- **102 Processing** indicates that the server has received the request and is processing it, but the response is not yet available
			- **2xx** *successful* - The request was successfully received, understood, and accepted
				- **200 OK** indicates that the request succeeded
				- **201 Created** indicates that the request succeeded, and a new resource was created (i.e. in response to POST)
			- **3xx** *redirection* - Further action needs to be taken in order to complete the request
				- 301 Moved Permanently indicates that the requested resource has been moved, and the server indicates its new location
			- **4xx** *client error* - The request contains bad syntax or cannot be fulfilled
				- **403 Unauthorized** means the client must authenticate to get a response
				- **404 Not Found** means the requested resource was not found
			- **5xx** *server error* - The server failed to fulfill an apparently valid request
				- **500 Internal Server Error** means the server encountered something unexpected that it doesn't know how to handle
- ## REST ^ccna-rest
	- REST stands for Representational State Transfer
	- **REST APIs** are also known as **REST-based APIs** or **RESTful APIs**
		- REST isn't a specific API, instead, it describes a set of rules about how the API should work
	- The six constraints of RESTful architecture are:
		- ### Uniform Interface
		- ### Client-server
			- **REST APIs** use a client-server architecture
			- The client uses API calls (HTTP requests) to access the resources on the server
			- The separation between the client and server means they can both change and evolve independently of each other
				- When the client application changes or the server application changes, the interface between them must not break
		- ### Stateless
			- **REST APIs** exchanges are stateless
			- This means that each API exchange is a separate event, independent of all past exchanges between the client and server
				- The server does not store new information about previous requests from the client to determine how it should respond to new requests
			- If authentication is required, this means that the client must authenticate with the server for each request it makes
			- TCP is an example of a stateful protocol
			- UDP is an example of a stateless protocol
		- ### Cacheable or Non-Cacheable
			- **REST APIs** must support caching of data
			- *Caching* refers to storing data for future use
				- For example, your computer might cache many elements of a web page so that it doesn't have to retrieve the entire page every time you visit it
				- This improves performance for the client and reduces the load on the server
			- Not all resources have to be cacheable, but cacheable resources MUST be declared as cacheable
		- ### Layered system
		- ### Code-on-demand (optional)
	- For applications to communicate over a network, networking protocols must be used to facilitate those communications
		- For REST APIs, HTTP(S) is the most common choice
- ## Cisco DevNet
	- Cisco DevNet is Cisco's developer program to help developers and IT professionals who want to write applications and develop integrations with Cisco products, platforms, and APIs
	- DevNet offers lots of free resources such as courses, tutorials, labs, sandboxes, documentation, etc. to learn about automation and develop your skills
	- There is also a DevNet certification track that you can pursue if you're interested in automation
	- You can use their Cisco DNA Center Sandbox to send a REST API call using Postman.
		- DNA Center is one of Cisco's SDN controllers.
		- Postman is a platform for building and using APIs.