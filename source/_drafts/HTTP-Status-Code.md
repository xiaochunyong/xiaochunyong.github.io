Status code
Defined range
100–101 200–206 300–305 400–415 500–505
APPENDIX B HTTP Status Codes
￼￼This appendix is a quick reference of HTTP status codes and their meanings.
Status Code Classifications
HTTP status codes are segmented into five classes, shown in Table B-1.
Table B-1. Status code classifications
100–199 Informational 200–299 Successful 300–399 Redirection 400–499 Client error 500–599 Server error
Status Codes
TableB-2isaquickreferenceforallthestatuscodesdefinedintheHTTP/1.1speci- fication, providing a brief summary of each. “Status Codes” in Chapter 3 goes into more detailed descriptions of these status codes and their uses.
￼￼￼￼Table B-2. Status codes
100 101
200 OK
201 Created
An initial part of the request was received, and the client should continue.
The server is changing protocols, as specified by the client, to one listed in the Upgrade header.
The request is okay.
The resource was created (for requests that create server objects).
￼￼￼Reason phrase
Continue Switching Protocols
Category
Meaning
￼￼505
￼Table B-2. Status codes (continued)
￼￼￼Status code
Reason phrase
Accepted
Non-Authoritative Information
No Content Reset Content Partial Content
Multiple Choices
Moved Permanently Found
See Other Not Modified Use Proxy
(Unused) Temporary Redirect
Meaning
202 203
204 205
206 300
301 302
303
304
305
306 307
400 401
402
403 404 405
406
407
Bad Request Unauthorized
Payment Required
Forbidden
Not Found
Method Not Allowed
Not Acceptable
Proxy Authentication Required
The request was accepted, but the server has not yet performed any action with it.
The transaction was okay, except the information contained in the entity headers was not from the origin server, but from a copy of the resource.
The response message contains headers and a status line, but no entity body.
Another code primarily for browsers; basically means that the browser should clear any HTML form elements on the current page.
A partial request was successful.
A client has requested a URL that actually refers to multiple resources. This code is returned along with a list of options; the user can then select which one he wants.
The requested URL has been moved. The response should contain a Location URL indicating where the resource now resides.
Like the 301 status code, but the move is temporary. The client should use the URL given in the Location header to locate the resource temporarily.
Tells the client that the resource should be fetched using a different URL. This new URL is in the Location header of the response message.
Clients can make their requests conditional by the request headers they include. This code indicates that the resource has not changed.
The resource must be accessed through a proxy, the location of the proxy is given in the Location header.
This status code currently is not used.
Like the 301 status code; however, the client should use the URL given in the Location header to locate the resource temporarily.
Tells the client that it sent a malformed request.
Returned along with appropriate headers that ask the client to authenticate itself before it can gain access to the resource.
Currently this status code is not used, but it has been set aside for future use.
The request was refused by the server. The server cannot find the requested URL.
A request was made with a method that is not supported for the requested URL. The Allow header should be included in the response to tell the client what methods are allowed on the requested resource.
Clients can specify parameters about what types of entities they are willing to accept. This code is used when the server has no resource matching the URL that is acceptable for the client.
Like the 401 status code, but used for proxy servers that require authentication for a resource.
￼￼￼506 |
Appendix B: HTTP Status Codes
Table B-2. Status codes (continued)
Status code
Reason phrase
Request Timeout
Conflict Gone
Length Required
Precondition Failed
Request Entity Too Large Request URI Too Long Unsupported Media Type Requested Range Not Satisfiable Expectation Failed
Internal Server Error
Not Implemented Bad Gateway
Service Unavailable Gateway Timeout
HTTP Version Not Supported
Meaning
408 If a client takes too long to complete its request, a server can send back this status code and close down the connection.
409 The request is causing some conflict on a resource.
410 Like the 404 status code, except that the server once held the resource.
411 Servers use this code when they require a Content-Length header in the request message. The server will not accept requests for the
resource without the Content-Length header.
412 If a client makes a conditional request and one of the conditions fails, this response code is returned.
413 The client sent an entity body that is larger than the server can or wants to process.
414 The client sent a request with a request URL that is larger than what the server can or wants to process.
415 The client sent an entity of a content type that the server does not understand or support.
416 The request message requested a range of a given resource, and that range either was invalid or could not be met.
417 The request contained an expectation in the Expect request header that could not be satisfied by the server.
500 The server encountered an error that prevented it from servicing the request.
501 The client made a request that is beyond the server’s capabilities.
502 A server acting as a proxy or gateway encountered a bogus response from the next link in the request response chain.
503 The server cannot currently service the request but will be able to in the future.
504 Similar to the 408 status code, except that the response is coming from a gateway or proxy that has timed out waiting for a response
to its request from another server.
505 The server received a request in a version of the protocol that it can’t or won’t support.