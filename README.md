# Library Management API

A RESTful API built with PHP Slim Framework for managing a library system with authors and books. The API includes user authentication with JWT tokens and CRUD operations for both authors and books.

## Features

- User authentication with JWT tokens
- Author management (CRUD operations)
- Book management (CRUD operations)
- Relationship management between authors and books
- Token-based security for protected endpoints

## Prerequisites

- PHP 7.0 or higher
- MySQL
- Composer
- PDO PHP Extension
- Following dependencies (installed via Composer):
  - Slim Framework
  - Firebase JWT
  - PSR-7 Implementation

## Database Setup

Create a MySQL database named 'library' with the following tables:

```sql
CREATE TABLE users (
    userid INT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(255) UNIQUE,
    password VARCHAR(255)
);

CREATE TABLE authors (
    authorid INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(255)
);

CREATE TABLE books (
    bookid INT AUTO_INCREMENT PRIMARY KEY,
    title VARCHAR(255),
    authorid INT,
    FOREIGN KEY (authorid) REFERENCES authors(authorid)
);

CREATE TABLE books_authors (
    bookid INT,
    authorid INT,
    FOREIGN KEY (bookid) REFERENCES books(bookid),
    FOREIGN KEY (authorid) REFERENCES authors(authorid),
    PRIMARY KEY (bookid, authorid)
);
```

## API Endpoints

### Authentication

- **POST /user/register**
  - Register a new user
  - Body: `{"username": "string", "password": "string"}`

- **POST /user/auth**
  - Authenticate user and get JWT token
  - Body: `{"username": "string", "password": "string"}`

### Authors

- **POST /user/author/create**
  - Create new author
  - Requires token
  - Body: `{"name": "string", "token": "string"}`

- **GET /user/author/read**
  - Get all authors
  - Requires token in Authorization header

- **PUT /user/author/update**
  - Update author
  - Requires token
  - Body: `{"authorid": number, "name": "string", "token": "string"}`

- **DELETE /user/author/delete**
  - Delete author
  - Requires token
  - Body: `{"authorid": number, "token": "string"}`

### Books

- **POST /user/book/create**
  - Create new book
  - Requires token
  - Body: `{"title": "string", "authorid": number, "token": "string"}`

- **GET /user/book/read**
  - Get all books
  - Requires token in Authorization header

- **PUT /user/book/update**
  - Update book
  - Requires token
  - Body: `{"id": number, "title": "string", "authorid": number, "token": "string"}`

- **DELETE /user/book/delete**
  - Delete book
  - Requires token
  - Body: `{"bookid": number, "token": "string"}`

### Additional Endpoints

- **GET /books**
  - Get all authors with their books
  - Requires token in Authorization header

## Authentication

The API uses JWT tokens for authentication. All protected endpoints require either:
- Token in the request body as `"token": "string"`
- Token in the Authorization header as `Authorization: Token <token>`

Tokens expire after 300 seconds (5 minutes) except for book updates which have a longer expiration time.

## Error Handling

The API returns responses in the following format:

```json
{
    "status": "success|fail",
    "data": null|object,
    "token": "string" (for endpoints that refresh tokens)
}
```

## Security Notes

1. Passwords are hashed using SHA-256 before storage
2. SQL injection is prevented using prepared statements
3. Token verification is required for all protected endpoints
4. Duplicate checks are implemented for both authors and books

## Local Development Setup

1. Clone the repository
2. Install dependencies:
   ```bash
   composer install
   ```
3. Configure your database connection in index.php:
   ```php
   $servername = "localhost";
   $username = "root";
   $password = "";
   $dbname = "library";
   ```
4. Start your local PHP server:
   ```bash
   php -S localhost:8000
   ```

## Error Reporting

The API has error reporting enabled (`error_reporting(E_ALL)`) for development purposes. Consider adjusting this for production environments.
