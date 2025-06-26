Start the application in development mode
npm run start:dev
Build for production
npm run build
Start production server
npm run start:prod

### Database Setup

The application uses TypeORM with synchronize enabled in development mode, so tables will be created automatically when you first run the application.

For production, you should:
1. Set `synchronize: false` in database config
2. Use migrations instead of synchronize
3. Run migrations with: `npm run migration:run`

## API Documentation

Once the application is running, you can access:
- **API Documentation (Swagger)**: http://localhost:3000/api
- **Base API URL**: http://localhost:3000

## API Endpoints & Examples

### 1. Users

#### Register a User
```bash
curl -X POST http://localhost:3000/users \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Chandini",
    "email": "chandini@example.com"
  }'
Response:
json{
  "id": "550e8400-e29b-41d4-a716-446655440000",
  "name": "Chandini",
  "email": "chandini@example.com",
  "createdAt": "2024-01-15T10:00:00.000Z"
}
Get All Users
bashcurl -X GET http://localhost:3000/users
Response:
json[
  {
    "id": "550e8400-e29b-41d4-a716-446655440000",
    "name": "Chandini",
    "email": "chandini@example.com",
    "createdAt": "2024-01-15T10:00:00.000Z"
  }
]
2. Services
Create a Service
bashcurl -X POST http://localhost:3000/services \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Doctor Consultation",
    "duration": 30
  }'
Response:
json{
  "id": "660e8400-e29b-41d4-a716-446655440001",
  "name": "Doctor Consultation",
  "duration": 30,
  "createdAt": "2024-01-15T10:05:00.000Z"
}
Get All Services
bashcurl -X GET http://localhost:3000/services
Response:
json[
  {
    "id": "660e8400-e29b-41d4-a716-446655440001",
    "name": "Doctor Consultation",
    "duration": 30,
    "createdAt": "2024-01-15T10:05:00.000Z"
  }
]
3. Appointments
Book an Appointment
bashcurl -X POST http://localhost:3000/appointments \
  -H "Content-Type: application/json" \
  -d '{
    "userId": "550e8400-e29b-41d4-a716-446655440000",
    "serviceId": "660e8400-e29b-41d4-a716-446655440001",
    "scheduledAt": "2024-01-20T14:30:00Z"
  }'
Response:
json{
  "id": "770e8400-e29b-41d4-a716-446655440002",
  "userId": "550e8400-e29b-41d4-a716-446655440000",
  "serviceId": "660e8400-e29b-41d4-a716-446655440001",
  "scheduledAt": "2024-01-20T14:30:00.000Z",
  "status": "booked",
  "createdAt": "2024-01-15T10:10:00.000Z"
}
Get All Appointments (with pagination)
bash# Basic request
curl -X GET http://localhost:3000/appointments

# With pagination
curl -X GET "http://localhost:3000/appointments?page=1&limit=10"

# Filter by status
curl -X GET "http://localhost:3000/appointments?status=booked&page=1&limit=5"
Response:
json{
  "data": [
    {
      "id": "770e8400-e29b-41d4-a716-446655440002",
      "userId": "550e8400-e29b-41d4-a716-446655440000",
      "serviceId": "660e8400-e29b-41d4-a716-446655440001",
      "scheduledAt": "2024-01-20T14:30:00.000Z",
      "status": "booked",
      "createdAt": "2024-01-15T10:10:00.000Z",
      "user": {
        "id": "550e8400-e29b-41d4-a716-446655440000",
        "name": "Chandini",
        "email": "chandini@example.com"
      },
      "service": {
        "id": "660e8400-e29b-41d4-a716-446655440001",
        "name": "Doctor Consultation",
        "duration": 30
      }
    }
  ],
  "total": 1,
  "page": 1,
  "limit": 10,
  "totalPages": 1
}
Cancel an Appointment
bashcurl -X PUT http://localhost:3000/appointments/770e8400-e29b-41d4-a716-446655440002/cancel
Response:
json{
  "id": "770e8400-e29b-41d4-a716-446655440002",
  "userId": "550e8400-e29b-41d4-a716-446655440000",
  "serviceId": "660e8400-e29b-41d4-a716-446655440001",
  "scheduledAt": "2024-01-20T14:30:00.000Z",
  "status": "cancelled",
  "createdAt": "2024-01-15T10:10:00.000Z",
  "user": {
    "id": "550e8400-e29b-41d4-a716-446655440000",
    "name": "Chandini",
    "email": "chandini@example.com"
  },
  "service": {
    "id": "660e8400-e29b-41d4-a716-446655440001",
    "name": "Doctor Consultation",
    "duration": 30
  }
}
Complete an Appointment
bashcurl -X PUT http://localhost:3000/appointments/770e8400-e29b-41d4-a716-446655440002/complete
Response:
json{
  "id": "770e8400-e29b-41d4-a716-446655440002",
  "userId": "550e8400-e29b-41d4-a716-446655440000",
  "serviceId": "660e8400-e29b-41d4-a716-446655440001",
  "scheduledAt": "2024-01-20T14:30:00.000Z",
  "status": "completed",
  "createdAt": "2024-01-15T10:10:00.000Z",
  "user": {
    "id": "550e8400-e29b-41d4-a716-446655440000",
    "name": "Chandini",
    "email": "chandini@example.com"
  },
  "service": {
    "id": "660e8400-e29b-41d4-a716-446655440001",
    "name": "Doctor Consultation",
    "duration": 30
  }
}
Error Responses
The API returns appropriate HTTP status codes and error messages:
400 Bad Request
json{
  "statusCode": 400,
  "message": [
    "name should not be empty",
    "email must be an email"
  ],
  "error": "Bad Request"
}
404 Not Found
json{
  "statusCode": 404,
  "message": "User not found",
  "error": "Not Found"
}
409 Conflict
json{
  "statusCode": 409,
  "message": "Email already exists",
  "error": "Conflict"
}
Testing
Unit Tests
bash# Run all tests
npm test

# Run tests in watch mode
npm run test:watch

# Run tests with coverage
npm run test:cov
End-to-End Tests
bashnpm run test:e2e
Database Schema
The application automatically creates the following tables:
users

id (UUID, Primary Key)
name (VARCHAR)
email (VARCHAR, Unique)
createdAt (TIMESTAMP)

services

id (UUID, Primary Key)
name (VARCHAR)
duration (INTEGER)
createdAt (TIMESTAMP)

appointments

id (UUID, Primary Key)
userId (UUID, Foreign Key → users.id)
serviceId (UUID, Foreign Key → services.id)
scheduledAt (TIMESTAMP)
status (ENUM: 'booked', 'cancelled', 'completed')
createdAt (TIMESTAMP)

Features Implemented
Core Requirements:

User registration and listing
Service creation and listing
Appointment booking, cancellation, and completion
Input validation using class-validator
Proper error handling with appropriate HTTP status codes
Environment configuration with .env
TypeScript and NestJS framework
PostgreSQL with TypeORM

Bonus Features:

Swagger API documentation
Pagination for appointment listing
Comprehensive error handling
Request/Response DTOs
Database relationships
Conflict detection for overlapping appointments

Project Structure Overview
src/
├── app.module.ts              
├── main.ts                   
├── config/
│   └── database.config.ts     
├── users/                    
│   ├── users.module.ts
│   ├── users.controller.ts
│   ├── users.service.ts
│   ├── user.entity.ts
│   └── dto/
│       └── create-user.dto.ts
├── services/                  
│   ├── services.module.ts
│   ├── services.controller.ts
│   ├── services.service.ts
│   ├── service.entity.ts
│   └── dto/
│       └── create-service.dto.ts
└── appointments/              
    ├── appointments.module.ts
    ├── appointments.controller.ts
    ├── appointments.service.ts
    ├── appointment.entity.ts
    ├── appointment-status.enum.ts
    └── dto/
        ├── create-appointment.dto.ts
        └── query-appointments.dto.ts
Git Workflow
Initial Setup
bashgit init
git add .
git commit -m "Initial project setup with NestJS and TypeORM"
Recommended Commit Messages
bashgit commit -m "feat: add user registration and listing endpoints"
git commit -m "feat: implement service creation and management"
git commit -m "feat: add appointment booking with conflict detection"
git commit -m "feat: implement appointment cancellation and completion"
git commit -m "feat: add pagination support for appointments listing"
git commit -m "docs: add Swagger API documentation"
git commit -m "test: add unit tests for users service"
git commit -m "fix: handle edge cases in appointment scheduling"
Deployment
Environment Variables for Production
envNODE_ENV=production
PORT=3000
DB_HOST=your-production-db-host
DB_PORT=5432
DB_USERNAME=your-production-db-user
DB_PASSWORD=your-production-db-password
DB_DATABASE=appointment_booking_prod
Build and Deploy
bash# Build the application
npm run build

# Start in production mode
npm run start:prod
Contributing

Fork the repository
Create a feature branch (git checkout -b feature/amazing-feature)
Commit your changes (git commit -m 'Add some amazing feature')
Push to the branch (git push origin feature/amazing-feature)
Open a Pull Request
