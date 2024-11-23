# Library Management System with JWT

A Library Management System designed for secure and efficient book and author management using JSON Web Tokens (JWT) for authentication. This system allows users to manage their library collection, ensuring only authorized access to CRUD operations. The system supports features like user registration, login, and managing books and authors through a RESTful API.

## Table of Contents
- [Features](#features)
- [Setup](#setup)
- [SQL Database](#sql-database)
- [Technologies Used](#technologies-used)
- [API Endpoints](#api-endpoints)
  - [Authentication](#authentication)
  - [Authors Management](#authors-management)
  - [Books Management](#books-management)
  - [Combined Operations](#combined-operations)

## Features
- User Registration and Authentication
- Token-Based Authentication with JWT
- CRUD Operations for:
  - Books
  - Authors
  - Users
- Secure Database Operations
- Relationship Management between Books and Authors

## Setup
1. Clone the repository:
```bash
git clone [your-repository-url]
cd [repository-name]
```

2. Install dependencies:
```bash
composer install
```

3. Configure the database:
```php
$servername = "localhost";
$username = "root";
$password = "";
$dbname = "library";
```

4. Set up the database schema:
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

5. Start your local PHP server:
```bash
php -S localhost:8000
```

## Technologies Used
- Backend: PHP with Slim Framework
- Database: MySQL
- Authentication: JWT (JSON Web Tokens)
- Dependencies: Firebase JWT, PSR-7 Implementation

## API Endpoints

### Authentication

#### Register Users
- **Endpoint:** `POST /user/register`
- **Request:**
```json
{
  "username": "User Name",
  "password": "username123"
}
```
- **Response:**
```json
{
  "status": "success",
  "data": null
}
```

#### Authenticate Users
- **Endpoint:** `POST /user/auth`
- **Request:**
```json
{
  "username": "User Name",
  "password": "username123"
}
```
- **Response:**
```json
{
  "status": "success",
  "token": "{{jwt-token}}",
  "data": null
}
```

### Authors Management

#### Create Author
- **Endpoint:** `POST /user/author/create`
- **Request:**
```json
{
  "name": "Author Name",
  "token": "{{jwt-token}}"
}
```

#### Read Authors
- **Endpoint:** `GET /user/author/read`
- **Headers:** `Authorization: Token {{jwt-token}}`

#### Update Author
- **Endpoint:** `PUT /user/author/update`
- **Request:**
```json
{
  "authorid": 1,
  "name": "New Author Name",
  "token": "{{jwt-token}}"
}
```

#### Delete Author
- **Endpoint:** `DELETE /user/author/delete`
- **Request:**
```json
{
  "authorid": 1,
  "token": "{{jwt-token}}"
}
```

### Books Management

#### Create Book
- **Endpoint:** `POST /user/book/create`
- **Request:**
```json
{
  "title": "Book Title",
  "authorid": 1,
  "token": "{{jwt-token}}"
}
```

#### Read Books
- **Endpoint:** `GET /user/book/read`
- **Headers:** `Authorization: Token {{jwt-token}}`

#### Update Book
- **Endpoint:** `PUT /user/book/update`
- **Request:**
```json
{
  "id": 1,
  "title": "New Book Title",
  "authorid": 1,
  "token": "{{jwt-token}}"
}
```

#### Delete Book
- **Endpoint:** `DELETE /user/book/delete`
- **Request:**
```json
{
  "bookid": 1,
  "token": "{{jwt-token}}"
}
```

### Combined Operations

#### Get Authors with Books
- **Endpoint:** `GET /books`
- **Headers:** `Authorization: Token {{jwt-token}}`
- **Response:**
```json
{
  "status": "success",
  "data": [
    {
      "author": "Author Name",
      "books": ["Book 1", "Book 2"]
    }
  ]
}
```

## Security Notes

1. Passwords are hashed using SHA-256
2. SQL injection prevention through prepared statements
3. Token verification required for protected endpoints
4. Duplicate checks for authors and books
5. Token expiration after 300 seconds (5 minutes)

## Error Handling

All API responses follow this format:
```json
{
  "status": "success|fail",
  "data": null|object,
  "token": "string" (for endpoints that refresh tokens)
}
```

## Error Reporting

The API has error reporting enabled (`error_reporting(E_ALL)`) for development. Consider adjusting this for production environments.
