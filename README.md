# Family Cash Card Service

An application for families to manage allowances in the form of digital debit cards.

## Technologies Used

- **Java 17** - Modern Java programming language
- **Spring Boot 3.2.0** - Application framework
- **Spring Security** - Authentication and authorization
- **Spring Data JDBC** - Database access
- **H2 Database** - In-memory database for testing
- **Gradle** - Build automation tool
- **JUnit 5** - Testing framework

## Features

- ✅ **REST API** - Full CRUD operations for cash cards
- ✅ **Test-Driven Development** - Comprehensive test coverage
- ✅ **Security** - Basic authentication with user-based access control
- ✅ **Pagination & Sorting** - Efficient data retrieval
- ✅ **Owner-based Authorization** - Users can only access their own cash cards

## REST API Endpoints

All endpoints require Basic Authentication.

### GET /cashcards/{id}
Get a specific cash card by ID (only if owned by authenticated user).

**Response:** 200 OK
```json
{
  "id": 99,
  "amount": 123.45,
  "owner": "sarah1"
}
```

### GET /cashcards
Get all cash cards owned by authenticated user with optional pagination and sorting.

**Query Parameters:**
- `page` - Page number (default: 0)
- `size` - Items per page (default: 20)
- `sort` - Sort field and direction (e.g., `amount,desc`)

**Response:** 200 OK
```json
[
  {
    "id": 99,
    "amount": 123.45,
    "owner": "sarah1"
  },
  {
    "id": 100,
    "amount": 1.00,
    "owner": "sarah1"
  }
]
```

### POST /cashcards
Create a new cash card for the authenticated user.

**Request Body:**
```json
{
  "amount": 250.00
}
```

**Response:** 201 Created
- Location header contains URI of created resource

### PUT /cashcards/{id}
Update an existing cash card (only if owned by authenticated user).

**Request Body:**
```json
{
  "amount": 19.99
}
```

**Response:** 204 No Content

### DELETE /cashcards/{id}
Delete a cash card (only if owned by authenticated user).

**Response:** 204 No Content

## Building and Running

### Prerequisites
- Java 17 or higher
- No need to install Gradle (uses Gradle wrapper)

### Build the project
```bash
./gradlew build
```

### Run tests
```bash
./gradlew test
```

### Run the application
```bash
./gradlew bootRun
```

The application will start on `http://localhost:8080`

## Testing the API

You can test the API using curl or any REST client. Example:

```bash
# Get a cash card
curl -u sarah1:abc123 http://localhost:8080/cashcards/99

# Create a new cash card
curl -X POST -u sarah1:abc123 \
  -H "Content-Type: application/json" \
  -d '{"amount": 250.00}' \
  http://localhost:8080/cashcards

# Update a cash card
curl -X PUT -u sarah1:abc123 \
  -H "Content-Type: application/json" \
  -d '{"amount": 19.99}' \
  http://localhost:8080/cashcards/99

# Delete a cash card
curl -X DELETE -u sarah1:abc123 \
  http://localhost:8080/cashcards/99

# List all cash cards with pagination
curl -u sarah1:abc123 \
  "http://localhost:8080/cashcards?page=0&size=10&sort=amount,desc"
```

## Test Users

⚠️ **Security Warning**: The following are test credentials for development only. Never use these in production!

The application includes the following test users:

| Username | Password | Role | Cards |
|----------|----------|------|-------|
| sarah1 | abc123 | CARD-OWNER | 99, 100 |
| kumar2 | xyz789 | CARD-OWNER | 101 |
| hank-owns-no-cards | qrs456 | NON-OWNER | none |

## Security Features

- **Authentication** - HTTP Basic Authentication (all endpoints require authentication)
- **Authorization** - Data-level ownership validation (users can only access their own cash cards)
- **CSRF Protection** - Disabled for REST API usage
- **Password Encryption** - BCrypt password encoding

## Architecture

The application follows a layered architecture:

1. **Controller Layer** (`CashCardController`) - Handles HTTP requests and responses
2. **Repository Layer** (`CashCardRepository`) - Data access using Spring Data JDBC
3. **Domain Model** (`CashCard`) - Java record representing the cash card entity
4. **Security Layer** (`SecurityConfig`) - Authentication and authorization configuration

## Test Coverage

The application includes comprehensive tests:

- **Unit Tests** - JSON serialization/deserialization tests
- **Integration Tests** - Full REST API endpoint tests with security
- **TDD Approach** - Tests written before implementation

## Future Enhancements

Potential improvements for the application:

- [ ] Add support for multiple family members per account
- [ ] Implement transaction history
- [ ] Add spending categories and limits
- [ ] Create a web UI
- [ ] Add email notifications
- [ ] Implement JWT authentication
- [ ] Add PostgreSQL/MySQL support for production