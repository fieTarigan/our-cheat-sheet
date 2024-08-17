# Example 1

### Endpoint: Find cash card

- **Method**: GET
- **URL**: `/cashcards/{id}`
- **Description**: Find specific cash card

#### Responses:
- **200 OK**
    - **Description**: if the user is authorized and the Cash Card was successfully retrieved
    - **Headers**: -
    - **Body**:
    ```json
    {
        "id": "integer",
        "amount": "float"
    }
    ```
- **401 UNAUTHORIZED**
    - **Description**: if the user is unauthenticated or unauthorized
    - **Headers**: -
    - **Body**: _not specified_
- **404 NOT FOUND**
    - **Description**: if the user is authenticated and authorized but the Cash Card cannot be found
    - **Headers**: -
    - **Body**: _not specified_

# Example 2

Here's an example of a detailed API contract for a single endpoint. This example assumes we're building an API for managing a collection of books in a library:

### Endpoint: Create a New Book

- **Method**: POST
- **URL**: `/api/v1/books`
- **Description**: Creates a new book in the library's collection.

#### Request Headers:
- **Content-Type**: `application/json`
- **Authorization**: `Bearer {token}`

#### Request Body:
```json
{
  "title": "string",
  "author": "string",
  "isbn": "string",
  "publishedDate": "YYYY-MM-DD",
  "pages": "integer",
  "language": "string",
  "publisher": "string"
}
```

#### Request Parameters:
- **None**: This endpoint does not require query parameters.

#### Responses:

- **201 Created**
  - **Description**: The book was successfully created.
  - **Headers**:
    - **Location**: `/api/v1/books/{id}`
  - **Body**:
  ```json
  {
    "id": "uuid",
    "title": "string",
    "author": "string",
    "isbn": "string",
    "publishedDate": "YYYY-MM-DD",
    "pages": "integer",
    "language": "string",
    "publisher": "string",
    "createdAt": "ISO8601 timestamp",
    "updatedAt": "ISO8601 timestamp"
  }
  ```

- **400 Bad Request**
  - **Description**: The request body is invalid.
  - **Body**:
  ```json
  {
    "error": "string",
    "message": "string",
    "details": [
      {
        "field": "string",
        "issue": "string"
      }
    ]
  }
  ```

- **401 Unauthorized**
  - **Description**: The user is not authenticated.
  - **Body**:
  ```json
  {
    "error": "Unauthorized",
    "message": "Authentication token is missing or invalid."
  }
  ```

- **403 Forbidden**
  - **Description**: The user does not have permission to perform this action.
  - **Body**:
  ```json
  {
    "error": "Forbidden",
    "message": "You do not have permission to create a book."
  }
  ```

- **500 Internal Server Error**
  - **Description**: An unexpected error occurred on the server.
  - **Body**:
  ```json
  {
    "error": "Internal Server Error",
    "message": "An unexpected error occurred. Please try again later."
  }
  ```

#### Example Usage:

**Request:**
```http
POST /api/v1/books HTTP/1.1
Host: api.example.com
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
Content-Type: application/json

{
  "title": "The Great Gatsby",
  "author": "F. Scott Fitzgerald",
  "isbn": "9780743273565",
  "publishedDate": "1925-04-10",
  "pages": 180,
  "language": "English",
  "publisher": "Charles Scribner's Sons"
}
```

**Response:**
```http
HTTP/1.1 201 Created
Location: /api/v1/books/550e8400-e29b-41d4-a716-446655440000
Content-Type: application/json

{
  "id": "550e8400-e29b-41d4-a716-446655440000",
  "title": "The Great Gatsby",
  "author": "F. Scott Fitzgerald",
  "isbn": "9780743273565",
  "publishedDate": "1925-04-10",
  "pages": 180,
  "language": "English",
  "publisher": "Charles Scribner's Sons",
  "createdAt": "2024-08-17T12:34:56Z",
  "updatedAt": "2024-08-17T12:34:56Z"
}
```

### Notes:
- **Authentication**: Requires a valid JWT token in the `Authorization` header.
- **Validation**:
  - `title`, `author`, `isbn`, `publishedDate`, `pages`, `language`, and `publisher` are required fields.
  - `isbn` must be unique.
  - `publishedDate` must be in `YYYY-MM-DD` format.
  - `pages` must be a positive integer.