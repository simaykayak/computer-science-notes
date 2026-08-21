# API Basics — REST & JSON

## 1. What is an API?

An **API (Application Programming Interface)** is a way for different software systems to communicate with each other.

Instead of allowing applications to directly access each other's internal systems, an API provides a defined interface through which they can request or send data.

For example, a weather application does not need to store weather information for every city itself. It can request this information from a weather service through an API.

```text
Application
     ↓
    API
     ↓
Server
     ↓
   JSON
     ↓
Application
```

A request could look like:

```http
GET /weather/prague
```

The server might respond with:

```json
{
  "city": "Prague",
  "temperature": 18,
  "condition": "Cloudy"
}
```

In this case, the API acts as the communication layer between the application and the server.

---

## 2. Why Do We Use APIs?

APIs allow different parts of a system to remain separated while still communicating with each other.

A typical web application can be represented as:

```text
Frontend
   ↓
  API
   ↓
Backend
   ↓
Database
```

The frontend normally does not communicate directly with the database.

Instead:

```text
Frontend → API → Backend → Database
```

This provides several advantages:

- **Security:** The database does not need to be directly exposed to the frontend.
- **Modularity:** Frontend and backend systems can be developed separately.
- **Reusability:** The same backend API can be used by web, mobile, and desktop applications.
- **Standardization:** Applications communicate through clearly defined requests and responses.

---

## 3. Client and Server

API communication usually involves two main sides:

### Client

The client sends a request.

Examples include:

- Web browsers
- Mobile applications
- Frontend applications
- Other backend services

### Server

The server receives the request, processes it, and sends a response.

```text
Client
   |
   | HTTP Request
   ↓
Server
   |
   | HTTP Response
   ↓
Client
```

For example:

```http
GET /users/5
```

The client is requesting the user whose ID is `5`.

The server could respond:

```json
{
  "id": 5,
  "name": "Alice"
}
```

---

## 4. HTTP

**HTTP (Hypertext Transfer Protocol)** is the protocol commonly used for communication between clients and servers on the web.

An HTTP request can contain:

```text
Method
URL
Headers
Body
```

For example:

```http
POST /users
Content-Type: application/json

{
  "name": "Alice"
}
```

Here:

```text
POST              → HTTP method
/users            → Endpoint
Content-Type      → Header
{"name":"Alice"}  → Request body
```

---

## 5. REST

**REST (Representational State Transfer)** is a common architectural style used when designing web APIs.

REST APIs organize systems around **resources**.

Examples of resources:

```text
users
products
orders
books
students
```

Resources are usually represented using endpoints:

```text
/users
/products
/orders
```

The action that should be performed is specified using an HTTP method.

For example:

```http
GET /users
```

is generally preferred over:

```text
/getUsers
```

because the HTTP method already describes the operation.

---

## 6. Endpoints

An **endpoint** is a specific address through which a client can access an API resource.

For example:

```text
https://api.example.com/users
```

The base API URL could be:

```text
https://api.example.com
```

while the endpoint is:

```text
/users
```

Other examples:

```text
/users
/users/5
/products
/products/17
/orders
```

---

## 7. HTTP Methods

### GET

`GET` is used to retrieve data.

Example:

```http
GET /users
```

This could return:

```json
[
  {
    "id": 1,
    "name": "Alice"
  },
  {
    "id": 2,
    "name": "Bob"
  }
]
```

To retrieve one specific user:

```http
GET /users/1
```

In simple terms:

```text
GET = Read
```

---

### POST

`POST` is commonly used to create a new resource.

```http
POST /users
```

Request body:

```json
{
  "name": "Simay",
  "email": "simay@example.com"
}
```

The server could respond:

```json
{
  "id": 3,
  "name": "Simay",
  "email": "simay@example.com"
}
```

In simple terms:

```text
POST = Create
```

---

### PUT

`PUT` is commonly used to replace or update an existing resource.

```http
PUT /users/3
```

Request body:

```json
{
  "name": "Simay Kayak",
  "email": "simay@example.com"
}
```

In simple terms:

```text
PUT = Update
```

Another method called `PATCH` is commonly used for partial updates.

```text
PUT   → Replace/update the complete resource
PATCH → Update selected fields
```

Example:

```http
PATCH /users/3
```

```json
{
  "name": "Simay Kayak"
}
```

---

### DELETE

`DELETE` removes a resource.

```http
DELETE /users/3
```

This means:

```text
Delete the user whose ID is 3.
```

---

## 8. CRUD

A fundamental concept in backend development is **CRUD**.

CRUD stands for:

| CRUD Operation | HTTP Method | Purpose |
|---|---|---|
| Create | POST | Create data |
| Read | GET | Retrieve data |
| Update | PUT / PATCH | Update data |
| Delete | DELETE | Remove data |

For example, a Books API could use:

```text
POST   /books       → Create a book
GET    /books       → Get all books
GET    /books/5     → Get book 5
PUT    /books/5     → Update book 5
DELETE /books/5     → Delete book 5
```

---

## 9. JSON

**JSON (JavaScript Object Notation)** is a lightweight data format commonly used to exchange information between clients and servers.

Example:

```json
{
  "name": "Simay",
  "age": 21,
  "student": true
}
```

JSON is both human-readable and easy for programs to process.

---

## 10. JSON Objects

A JSON object is surrounded by `{ }`.

```json
{
  "name": "Simay",
  "age": 21
}
```

JSON data uses:

```text
key : value
```

For example:

```json
"name": "Simay"
```

Here:

```text
name  → key
Simay → value
```

---

## 11. JSON Data Types

JSON supports several basic data types.

### String

```json
{
  "name": "Simay"
}
```

### Number

```json
{
  "age": 21
}
```

### Boolean

```json
{
  "student": true
}
```

### Null

```json
{
  "middleName": null
}
```

### Array

```json
{
  "languages": ["C", "Java", "C++"]
}
```

### Object

```json
{
  "student": {
    "name": "Simay",
    "age": 21
  }
}
```

---

## 12. JSON Arrays

JSON arrays use `[ ]`.

```json
[
  "C",
  "Java",
  "C++"
]
```

Arrays can also contain objects:

```json
[
  {
    "id": 1,
    "name": "Alice"
  },
  {
    "id": 2,
    "name": "Bob"
  }
]
```

This structure is commonly returned when requesting multiple resources from an API.

---

## 13. Nested JSON

JSON structures can be nested.

```json
{
  "id": 1,
  "name": "Simay",
  "university": {
    "name": "CTU",
    "city": "Prague"
  },
  "languages": [
    "C",
    "Java"
  ]
}
```

Here:

- `university` is another JSON object.
- `languages` is an array.

---

## 14. Designing a Simple Endpoint

Consider a simple book tracking application.

The resource is:

```text
books
```

The main endpoint could be:

```text
/api/books
```

Retrieve all books:

```http
GET /api/books
```

Retrieve one book:

```http
GET /api/books/5
```

Create a book:

```http
POST /api/books
```

Request body:

```json
{
  "title": "1984",
  "author": "George Orwell"
}
```

Update the book:

```http
PUT /api/books/5
```

Delete the book:

```http
DELETE /api/books/5
```

Together, these endpoints provide basic CRUD functionality.

---

## 15. URL Parameters

A value can be included directly in an endpoint to identify a specific resource.

```text
/users/5
```

The generic structure could be represented as:

```text
/users/{id}
```

For example:

```text
/users/12
```

means:

```text
id = 12
```

---

## 16. Query Parameters

Query parameters can be used for filtering, searching, sorting, pagination, or other optional information.

Example:

```text
/products?category=laptop
```

Here:

```text
category = laptop
```

Multiple parameters can be combined:

```text
/products?category=laptop&brand=apple
```

The `?` begins the query string.

The `&` separates multiple parameters.

---

## 17. Request Body

Methods such as POST, PUT, and PATCH often send data through the **request body**.

For example:

```http
POST /users
```

Request body:

```json
{
  "name": "Simay",
  "age": 21
}
```

The backend reads this JSON data and can use it to create a new user.

---

## 18. HTTP Response

After processing a request, the server sends an HTTP response.

A response commonly contains:

```text
Status Code
Headers
Body
```

Example:

```http
200 OK
Content-Type: application/json
```

Response body:

```json
{
  "id": 5,
  "name": "Simay"
}
```

---

## 19. HTTP Status Codes

Status codes describe the result of an HTTP request.

### 2xx — Success

```text
200 OK
```

The request was successful.

```text
201 Created
```

A new resource was successfully created.

```text
204 No Content
```

The operation succeeded but there is no response body.

### 4xx — Client Errors

```text
400 Bad Request
```

The request or provided data is invalid.

```text
401 Unauthorized
```

Authentication is required or invalid.

```text
403 Forbidden
```

The client is authenticated but does not have permission to perform the operation.

```text
404 Not Found
```

The requested resource does not exist.

### 5xx — Server Errors

```text
500 Internal Server Error
```

An unexpected error occurred on the server.

A useful way to remember the categories:

```text
2xx → Success
4xx → Client-side problem
5xx → Server-side problem
```

---

## 20. Basic API Flow

Consider a user registration process.

```text
1. User fills in a form
        ↓
2. Frontend sends a POST request
        ↓
3. JSON data is sent
        ↓
4. Backend reads and validates the data
        ↓
5. Backend stores the data in the database
        ↓
6. Server returns a response
```

Example request:

```http
POST /users
```

```json
{
  "name": "Simay",
  "email": "simay@example.com"
}
```

Example response:

```http
201 Created
```

```json
{
  "id": 27,
  "name": "Simay",
  "email": "simay@example.com"
}
```

---

## 21. Frontend, Backend, and Database

A typical modern application follows this structure:

```text
             HTTP Request
Frontend --------------------> Backend API
                                   |
                                   ↓
                               Database
                                   |
                                   ↓
Frontend <-------------------- Backend API
             JSON Response
```

For example:

```http
GET /api/students/10
```

The backend searches the database for the student and could return:

```json
{
  "id": 10,
  "name": "Simay",
  "department": "Software Engineering"
}
```

The frontend then uses this information to display the student data.

---

## 22. REST Endpoint Design

REST endpoints should usually represent resources rather than actions.

Preferred:

```text
/users
/users/5
/users/5/orders
/products
/products/12
```

Less REST-like:

```text
/getAllUsers
/createNewUser
/deleteUserById
```

The HTTP method already describes the action:

```http
GET /users
POST /users
DELETE /users/5
```

---

## 23. Real-World Example

Consider a food delivery application.

Retrieve restaurants:

```http
GET /restaurants
```

Retrieve one restaurant:

```http
GET /restaurants/7
```

Retrieve its menu:

```http
GET /restaurants/7/menu
```

Create an order:

```http
POST /orders
```

```json
{
  "restaurantId": 7,
  "items": [
    {
      "productId": 21,
      "quantity": 2
    }
  ]
}
```

Retrieve an order:

```http
GET /orders/125
```

Delete or cancel an order:

```http
DELETE /orders/125
```

Real backend systems use the same fundamental ideas, although their architecture is usually much larger and more complex.

---

## 24. API and Function Analogy

An API endpoint can loosely be thought of as a function that is available over a network.

For example, in C:

```c
int getUser(int id);
```

A similar API operation could be:

```http
GET /users/5
```

The input is effectively:

```text
id = 5
```

and the API returns data:

```json
{
  "id": 5,
  "name": "Alice"
}
```

This is not exactly how an API works internally, but it is a useful mental model when first learning backend development.

---

## 25. Complete REST API Example

A simple Students API could contain:

```text
GET    /api/students
GET    /api/students/5
POST   /api/students
PUT    /api/students/5
DELETE /api/students/5
```

Example POST request:

```json
{
  "name": "Simay",
  "department": "Software Engineering"
}
```

Example response:

```json
{
  "id": 5,
  "name": "Simay",
  "department": "Software Engineering"
}
```

This small example contains most of the fundamental concepts behind a basic REST API.

---

## 26. Summary

```text
API
↓
Allows different software systems to communicate.

REST
↓
A common architectural style for designing web APIs.

Endpoint
↓
A specific address inside an API.

GET
↓
Retrieve data.

POST
↓
Create new data.

PUT / PATCH
↓
Update existing data.

DELETE
↓
Remove data.

JSON
↓
A common format for exchanging structured data.

HTTP Status Code
↓
Describes the result of an HTTP request.

CRUD
↓
Create, Read, Update, Delete.
```

The general backend communication flow is:

```text
Frontend
   ↓
HTTP Request
   ↓
REST API
   ↓
Backend Logic
   ↓
Database
   ↓
JSON Response
   ↓
Frontend
```