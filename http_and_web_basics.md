# HTTP & Web Basics

## 1. Introduction

HTTP (Hypertext Transfer Protocol) is a protocol used for communication between clients and servers.

When we open a website or use an application, the client sends a request to the server. The server processes the request and sends back a response.

In this report, I learned about HTTP requests, HTTP responses, status codes, and CORS.

## 2. HTTP Request

An HTTP request is a message sent from a client to a server.

For example, when we open a website, the browser sends an HTTP request to get the webpage.

An HTTP request usually contains:

- **Method:** What action the client wants to perform.
- **URL:** The address of the requested resource.
- **Headers:** Additional information about the request.
- **Body:** Data sent to the server, if needed.

### Common HTTP Methods

| Method | Description |
|---|---|
| GET | Retrieves data from the server. |
| POST | Sends data to the server. |
| PUT | Replaces an existing resource. |
| PATCH | Updates part of a resource. |
| DELETE | Removes a resource. |

**Example:**

```http
GET /users HTTP/1.1
Host: example.com
```

This request asks the server for a list of users.

## 3. HTTP Response

An HTTP response is a message sent from the server to the client after receiving a request.

An HTTP response usually contains:

- **Status Code:** Shows the result of the request.
- **Headers:** Additional information about the response.
- **Body:** The returned data, if any.

**Example:**

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
  "id": 1,
  "name": "Alice"
}
```

This response means that the request was successful and the server returned user information.

## 4. HTTP Status Codes

HTTP status codes tell the client what happened after sending a request.

There are five main categories:

| Category | Meaning |
|---|---|
| 1xx | Informational |
| 2xx | Success |
| 3xx | Redirection |
| 4xx | Client Error |
| 5xx | Server Error |

### Common Status Codes

| Code | Name | Meaning |
|---|---|---|
| 200 | OK | The request was successful. |
| 201 | Created | A new resource was created. |
| 204 | No Content | The request succeeded without returning content. |
| 301 | Moved Permanently | The resource has a new permanent URL. |
| 400 | Bad Request | The server cannot process the invalid request. |
| 401 | Unauthorized | Authentication is required or invalid. |
| 403 | Forbidden | The client does not have permission. |
| 404 | Not Found | The requested resource does not exist. |
| 500 | Internal Server Error | An unexpected server error occurred. |
| 503 | Service Unavailable | The server is temporarily unavailable. |

**Example:**

If a user tries to open a page that does not exist, the server may return:

```http
HTTP/1.1 404 Not Found
```

## 5. CORS (Cross-Origin Resource Sharing)

CORS is a browser security mechanism that controls whether a webpage can access resources from another origin.

An origin is defined by its protocol, domain, and port.

For example:

- Frontend: `http://localhost:3000`
- Backend: `http://localhost:8080`

These are different origins because they use different ports.

If the frontend sends a request to the backend, the browser checks whether the backend allows this cross-origin access.

### How CORS Works

1. The frontend sends a request to the backend.
2. The backend sends a response with CORS headers.
3. The browser checks these headers.
4. If the origin is allowed, the frontend can access the response.
5. Otherwise, the browser blocks access to the response.

For some requests, the browser first sends a preflight request using the `OPTIONS` method.

**Example CORS Header:**

```http
Access-Control-Allow-Origin: http://localhost:3000
```

This tells the browser that the specified origin is allowed to access the response.

CORS is enforced by browsers. It is not a replacement for authentication or authorization.

## 6. Frontend and Backend Communication

Frontend and backend applications communicate using HTTP requests and responses.

**Example: User Login**

1. The user enters their username and password.
2. The frontend sends a POST request to the backend.
3. The backend checks the login information.
4. The backend sends an HTTP response.
5. The frontend displays the result.

**Example Request:**

```http
POST /login HTTP/1.1
Host: example.com
Content-Type: application/json

{
  "username": "alice",
  "password": "example123"
}
```

**Example Response:**

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
  "message": "Login successful"
}
```

This is a simple example of how frontend and backend systems exchange information.