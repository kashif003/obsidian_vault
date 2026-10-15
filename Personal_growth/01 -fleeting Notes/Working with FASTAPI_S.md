
---

#fastapi #API #api #FASTAPI

link: [youtube link](https://www.youtube.com/watch?v=WJKsPchji0Q&list=PLKnIA16_RmvZ41tjbKB2ZnwchfniNsMuQ)

CONTENT
1. Fundamentals of Fast API.
2. Connection with ML.
3. Deployment of API with AWS.


API: Application programming interface

---
# Fundamentals of Fast API.


<span style="color: var(--color-red)">what is API?</span>
	mechanism that enables two software components to communicate with  each other.

<span style="color: var(--color-red)">why do we need API?</span>
	+ TO overcome the limitations of monolithic architecture.
	+ Frontend is connected to backend with API, which allows the backend to connect to any request.
	+ Front end is connected to backend with API.


when the connection is through internet the protocol used is HTTP and the format used to communicate is JSON.


<span style="color: var(--color-yellow)">API's in ML-DL</span>:
	+ Backend of trained model is connected to frontend using the API.

---

<span style="color: var(--color-red)">what is FastAPI?</span>
	+ web framework for building API.
	+ made by using starlette and pydantic.
		+ starlette is responsible for API receives request and send back the response.
		+ data validation is performed using pydantic.

#SGI
SGI: server gateway interface, converts the requests into the python understandable format.

<span style="color: var(--color-red)">why we use FastAPI?
</span>
	+ APi's in fast to run and are Fast to code.
	+ Flask uses WSGI which is of synchronous nature(one by one) because of which it is slow and werkzeug is used as a library.
	+ while as Flask used ASGI which is Asynchronous (handles multiple requests) and uses uvicorn and also supports async and await feature of python.


#uvicorn
uvicorn: it is the server that we use.

---

#httpmethods #http
HTTTP methods:
	+ we have two kinds of  software: 
		1. static e.g calendar one way communication
		2. Dynamic e.g ms excel two way communication
	+ CRUD-> create, retrieve, update, delete
	+ CRUD-> GET, POST, PUT, DELETE

---

<span style="color: var(--color-yellow)">Path parameters</span>:
	+ Dynamic segments of the url path used to identify specific resource. e.g  ..../view/**3**

<span style="color: var(--color-yellow)">Path function</span>:
	+ it is used to provide the meta data, validation rules and documentation hints for the path parameter in API.

#statuscode
<span style="color: var(--color-yellow)">HTTP status codes</span>:°
	+ three digit code which indicate the result of a clients request.
	+ Four types:
		+ 2xx: success
		+ 3xx: redirection
		+ 4xx: client error
		+ 5xx: server error

<span style="color: var(--color-yellow)">HTTP exception</span>:
	+ build in exceptions  which return the HTTP error responses.

<span style="color: var(--color-yellow)">Query Parameter</span>:
	+ pass additional data stored in k,v pairs, used for filtering or sorting or another such requests
	+ multiple Params are separated with &.


---

[[pydantic crash course]]

----

How to use POST request?
	+ Request body: portion of HTTP request that contains data sent by the client to server using HTTP method using POST or PUT.
	+ validate using pydantics
	+ add to DB 
+ Mostly practical

---


How to update and delete?
	+ we will be using PUT request to update the BD.
	+ Need to make new pydantic model.


+ mostly practical stuff.

---

How to use ML model using FASTAPI?


+ practical section
+ downloaded the trained model now we are going to make APIS
+ we will be transforming raw data.
