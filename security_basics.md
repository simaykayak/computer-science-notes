# Security Basics

## 1. Introduction

Security is an important part of software development. A program should not only work correctly, but also protect user information and prevent unauthorized access.

In this report, I learned about authentication, authorization, JWT, OAuth, SQL Injection, and XSS.

## 2. Authentication vs Authorization

Authentication and authorization are two different security concepts.

### Authentication

Authentication means checking **who the user is**.

For example:
- Logging in with a username and password.
- Using a fingerprint to unlock a phone.
- Entering a verification code.

### Authorization

Authorization means checking **what the user is allowed to do**.

For example:
- A student can view their grades.
- A teacher can enter grades.
- An administrator can manage user accounts.

**Main difference:**

- Authentication: Who are you?
- Authorization: What can you do?

## 3. JWT (JSON Web Token)

JWT is a token format commonly used for authentication in web applications.

After a user logs in, the server can create a signed token. The client sends this token with future requests.

A JWT has three parts:

1. Header
2. Payload
3. Signature

**Example:**

1. The user logs in.
2. The server checks the login information.
3. The server creates a JWT.
4. The client sends the JWT with requests.
5. The server validates the token.

JWT helps applications manage authenticated requests. However, tokens must be protected because someone who steals a valid token may be able to use it.

## 4. OAuth

OAuth is a protocol that allows applications to access certain resources without sharing the user's password.

For example, an application may ask permission to access information from a Google account.

**Example:**

1. The user clicks "Continue with Google".
2. Google asks the user to sign in and approve the requested access.
3. The application receives an authorization code.
4. The application exchanges the code for a token.
5. The application uses the token to access permitted resources.

OAuth is mainly used for authorization. OpenID Connect (OIDC) adds an identity layer for user login.

## 5. SQL Injection

SQL Injection is a security attack where an attacker tries to change a database query by entering malicious SQL code.

This can happen when an application puts user input directly into an SQL query.

**Example of unsafe code:**

```sql
SELECT * FROM users
WHERE username = 'admin' AND password = '1234';
```

If user input is added directly to a query, an attacker may be able to change its meaning.

**How to prevent SQL Injection:**

- Use prepared statements.
- Use parameterized queries.
- Validate user input.
- Give database users only the permissions they need.

## 6. XSS (Cross-Site Scripting)

XSS is a security vulnerability where an attacker can make a website execute malicious JavaScript in another user's browser.

For example, a website allows users to write comments. If the website displays comments as HTML without proper protection, an attacker may insert a script.

**Example:**

```html
<script>alert("Hello")</script>
```

This simple example displays a message, but real XSS attacks can be more dangerous.

**How to prevent XSS:**

- Escape user-generated content when displaying it.
- Avoid inserting untrusted HTML.
- Use safe frameworks and APIs.
- Use Content Security Policy (CSP) as additional protection.
