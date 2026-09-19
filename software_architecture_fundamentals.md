# Software Architecture Fundamentals

## 1. Introduction

Software architecture describes the overall structure of a software system. It defines how different parts of an application are organized, what responsibilities they have, and how they communicate with each other.

In a small program, keeping most of the code in one or two files may be enough. However, when an application becomes larger, this approach becomes difficult to maintain. For example, a real application may contain authentication, database operations, user interfaces, business rules, APIs, and many other components.

One of the most important ideas in software architecture is **Separation of Concerns**. This means that different responsibilities should be handled by different parts of the application.

Good software architecture helps improve:

- **Maintainability** – making the software easier to modify and fix
- **Scalability** – allowing the system to handle growth
- **Testability** – making individual components easier to test
- **Readability** – making the structure easier for developers to understand

---

# 2. Monolithic Architecture

A **monolithic architecture** organizes an application mainly as one deployable system.

For example, an online shopping application may contain:

```text id="ftd6m8"
Online Shop
│
├── User Management
├── Products
├── Shopping Cart
├── Orders
└── Payments
```

Although these features can be separated into different classes, modules, or folders, they are still part of the same main application and are usually deployed together.

A monolith does not mean that all code must be written in one file. A monolithic project can still have a well-organized internal structure.

### Advantages

Monolithic applications are usually easier to start developing because there are fewer infrastructure requirements. Communication between different parts is also simple because they exist inside the same application.

They are generally easier to deploy and debug, especially for small and medium-sized projects.

### Disadvantages

As a monolithic application becomes very large, its codebase may become harder to understand and maintain.

Different modules can also become strongly dependent on each other if the architecture is not designed carefully.

Scaling can also be less flexible. If only one feature receives heavy traffic, it may still be necessary to scale a larger part of the application.

---

# 3. Microservice Architecture

A **microservice architecture** divides a large application into multiple smaller services.

For example:

```text id="mvef71"
Online Shopping System

├── User Service
├── Product Service
├── Order Service
├── Payment Service
└── Notification Service
```

Each service is responsible for a particular business capability and can usually be developed and deployed independently.

The services communicate over a network using technologies such as:

- REST APIs
- HTTP
- gRPC
- message queues

For example:

```text id="a2yrq6"
Order Service
      ↓
Payment Service
      ↓
Notification Service
```

When a customer creates an order, the Order Service may communicate with the Payment Service. After payment succeeds, the Notification Service may send an email to the customer.

### Advantages

One important advantage is **independent deployment**. A team may update the Payment Service without redeploying the entire system.

Another advantage is **independent scaling**. If the Product Service receives significantly more traffic, additional instances of only that service can be created.

Microservices can also make it easier for large development teams to divide responsibilities.

### Disadvantages

The main disadvantage is complexity.

In a monolith, one component may simply call another function. In microservices, communication happens over a network.

```text id="ocjepk"
Service A
   ↓
Network
   ↓
Service B
```

Networks can fail, services can become unavailable, and requests can time out.

Developers may therefore need to handle additional problems such as:

- service communication
- retries and timeouts
- authentication between services
- monitoring
- logging
- data consistency
- deployment of many services

For this reason, microservices are not automatically better than monoliths. For many smaller projects, a well-designed monolith is simpler and more practical.

---

# 4. Monolith vs Microservices

The main differences can be summarized as follows:

| Monolith | Microservices |
|---|---|
| One main application | Multiple independent services |
| Simpler to develop initially | More infrastructure complexity |
| Usually easier to deploy | Services can be deployed separately |
| Internal communication is simple | Communication often uses networks/APIs |
| Application may be scaled together | Individual services can be scaled |
| Suitable for many small/medium projects | Useful for some large systems |

The correct choice depends on the size, requirements, team structure, and expected growth of the project.

---

# 5. MVC – Model View Controller

**MVC** stands for **Model–View–Controller**. It is an architectural pattern that separates an application into three main responsibilities.

### Model

The **Model** represents application data and domain concepts.

For example, in a student management application:

```text id="00a9m9"
Student

id
name
email
grade
```

### View

The **View** is responsible for displaying information to the user.

It can include:

- pages
- forms
- tables
- buttons
- other interface elements

The View focuses mainly on how information is presented.

### Controller

The **Controller** receives user actions or requests and coordinates the appropriate application behavior.

For example:

```text id="tb3wmq"
User requests student information
             ↓
        Controller
             ↓
           Model
             ↓
           View
             ↓
           User
```

The important idea is that user-interface code, application behavior, and data-related responsibilities should not all be mixed together.

---

# 6. MVVM – Model View ViewModel

**MVVM** stands for **Model–View–ViewModel**.

Like MVC, its purpose is to separate the user interface from the rest of the application.

The main structure is:

```text id="hqp5en"
Model
  ↕
ViewModel
  ↕
View
```

The **Model** represents application data.

The **View** represents what the user sees.

The **ViewModel** provides data and operations that the View can use.

For example:

```text id="r65mpn"
StudentViewModel

studentName
studentGrade
isLoading
loadStudent()
deleteStudent()
```

Suppose:

```text id="xvyw4i"
isLoading = true
```

The View can display a loading indicator.

When the operation finishes:

```text id="4a6m20"
isLoading = false
```

the View can update accordingly.

MVVM is therefore particularly useful in applications where the user interface frequently reacts to changing data.

---

# 7. MVC vs MVVM

The main conceptual difference is the role of the **Controller** and **ViewModel**.

In MVC:

```text id="6dt1bv"
View
 ↓
Controller
 ↓
Model
```

The Controller handles requests or actions and coordinates what should happen.

In MVVM:

```text id="c6xykm"
View
 ↕
ViewModel
 ↕
Model
```

The ViewModel exposes state and operations to the View.

Both patterns follow the same general architectural idea: **different responsibilities should be separated instead of putting everything together.**

---

# 8. State Management

Another important concept in software architecture is **state**.

State describes the current condition of an application at a particular moment.

For example:

```text id="56aqqs"
loggedIn = true
cartItems = 4
darkMode = false
isLoading = true
selectedProduct = 25
```

If a user adds another product to the shopping cart:

```text id="6cd71c"
cartItems = 4
```

changes to:

```text id="fxeebx"
cartItems = 5
```

The application's state has changed.

**State management** is the process of controlling how state is stored, modified, and shared throughout an application.

This becomes especially important when several parts of the application need the same information.

For example:

```text id="4d2rxp"
            Shopping Cart
                  |
        --------------------
        |        |         |
      Header   Product   Checkout
```

The Header may display the number of products, the Product page may add products, and the Checkout page may display the full cart.

All of these components should work with consistent information.

---

# 9. Local and Global State

State can have different scopes.

### Local State

Local state belongs to one specific component.

For example:

```text id="qf59op"
isMenuOpen = true
```

Only the menu component may need this information.

Another example is:

```text id="m3t4d9"
passwordVisible = false
```

Only the password field needs to know this value.

### Global or Shared State

Some state needs to be accessed by many parts of the application.

Examples include:

```text id="of1nl9"
currentUser
authenticationStatus
shoppingCart
applicationTheme
```

For example:

```text id="s06ihp"
          Current User
               |
      -------------------
      |        |        |
    Header   Profile  Settings
```

State management helps ensure that all relevant parts of the application have consistent and updated information.

---

# 10. Layered Architecture

A common approach for organizing backend applications is **layered architecture**.

A typical structure is:

```text id="k69pqa"
Controller
     ↓
Service
     ↓
Repository
     ↓
Database
```

Each layer has a specific responsibility.

---

## Controller Layer

The **Controller** handles incoming requests.

For example:

```text id="fmxuzp"
GET /students/42
```

The Controller receives the request and calls the appropriate Service.

Its main responsibility can be summarized as:

```text id="r0dt57"
Receive request
      ↓
Call Service
      ↓
Return response
```

The Controller should generally not contain large amounts of business logic or database code.

---

## Service Layer

The **Service** contains or coordinates the application's business logic.

For example, suppose a student wants to register for a course.

Before registration, the system may need to check:

```text id="rzbs8x"
Does the student exist?

Does the course exist?

Is the course full?

Does the student meet the prerequisites?
```

These are business rules and are normally handled in the Service layer.

The Controller does not need to know exactly how these rules work. It only asks the Service to perform the operation.

---

## Repository Layer

The **Repository** handles data access.

Typical repository operations include:

```text id="1xx8ha"
findById()
findAll()
save()
update()
delete()
```

For example:

```text id="6btgvu"
StudentService
      ↓
StudentRepository
      ↓
Database
```

The Service does not need to contain SQL queries or know every detail about how the data is stored.

This makes it easier to separate business logic from database operations.

---

# 11. Example of the Complete Flow

Suppose the client sends:

```text id="p0wprh"
GET /students/42
```

The request travels through the system:

```text id="lhb0p2"
Client
  ↓
StudentController
  ↓
StudentService
  ↓
StudentRepository
  ↓
Database
```

The Repository retrieves the student from the database.

The result then travels back:

```text id="uvf0nk"
Database
  ↓
Repository
  ↓
Service
  ↓
Controller
  ↓
Client
```

The client might finally receive:

```json id="pq3sc9"
{
  "id": 42,
  "name": "Anna",
  "grade": "A"
}
```

Each layer has handled only its own responsibility.

---

# 12. Why Use Layers?

Without layers, a Controller could contain everything:

```text id="gdh3hd"
HTTP requests
+
business rules
+
validation
+
SQL queries
+
database operations
```

This may work initially, but it becomes difficult to maintain as the application grows.

With layered architecture:

```text id="v59g47"
Controller → request/response handling

Service → business logic

Repository → data access

Database → persistent storage
```

The responsibilities are clearly separated.

This improves maintainability and makes individual parts easier to test and modify.

---

# 13. How These Concepts Work Together

An important thing I learned is that these concepts describe different aspects of architecture.

For example, an application can be a **monolith** while internally using:

```text id="rfczcz"
Controller
    ↓
Service
    ↓
Repository
    ↓
Database
```

Similarly, a microservice can also use layered architecture internally.

For example:

```text id="rjy1pr"
Microservice A

Controller
   ↓
Service
   ↓
Repository
```

Therefore:

- **Monolith vs Microservices** describes how the overall system is divided and deployed.
- **MVC/MVVM** describes how responsibilities, especially around the user interface, can be separated.
- **State management** describes how changing application data is controlled.
- **Layered architecture** separates request handling, business logic, and data access.

---

# 14. Coupling and Cohesion

Two concepts closely related to architecture are **coupling** and **cohesion**.

**Coupling** describes how dependent different components are on each other.

If changing one component requires changes in many other components, the system has high coupling.

**Cohesion** describes how closely related the responsibilities inside one component are.

For example, a `PaymentService` should mainly contain responsibilities related to payments rather than unrelated operations.

A common architectural goal is:

```text id="n5kyuj"
Low Coupling
+
High Cohesion
```

This helps keep components independent and focused on their own responsibilities.

---
