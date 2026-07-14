# TaskFlow - Task Management API

A robust REST API for task management with JWT authentication, role-based access control (RBAC), audit logging, and automated email reminders.

## Features

- **JWT Authentication**: Secure token-based authentication
- **Role-Based Access Control**: 3 role types (Admin, Project Manager, Employee)
- **Task Management**: Create, read, update, delete tasks with status tracking
- **Project Management**: Organize tasks into projects
- **Audit Logging**: Track all user actions and changes
- **Email Reminders**: Automated cron-based task reminders
- **Database Seeding**: Sample data generation for testing
- **Error Handling**: Comprehensive error handling with meaningful messages
- **Rate Limiting**: API rate limiting for security
- **CORS Support**: Cross-Origin Resource Sharing enabled

## Tech Stack

- **Runtime**: Node.js
- **Framework**: Express.js
- **Database**: MySQL with Sequelize ORM
- **Authentication**: JWT (jsonwebtoken)
- **Password Hashing**: bcryptjs
- **Validation**: Joi
- **Cron Jobs**: node-cron
- **Email**: nodemailer
- **Security**: helmet, CORS, rate-limiting

## Installation

### Prerequisites

- Node.js (v14 or higher)
- MySQL (v5.7 or higher)
- npm or yarn

### Setup

1. **Clone the repository**
   ```bash
   git clone https://github.com/Sravani0703/TaskFlow.git
   cd TaskFlow
   ```

2. **Install dependencies**
   ```bash
   npm install
   ```

3. **Configure environment variables**
   ```bash
   cp .env.example .env
   # Edit .env with your configuration
   ```

4. **Create database**
   ```bash
   mysql -u root -p
   CREATE DATABASE taskflow_db;
   EXIT;
   ```

5. **Run migrations**
   ```bash
   npm run migrate
   ```

6. **Seed the database (optional - for sample data)**
   ```bash
   npm run seed
   ```

## Running the Application

### Development
```bash
npm run dev
```

### Production
```bash
NODE_ENV=production npm start
```

The API will be available at http://localhost:3000

### Docker Setup

To run the application using Docker:
```bash
docker-compose up --build
```

The API will be available at http://localhost:3000 and MySQL at localhost:3306

## API Documentation

### Authentication

**Register**
```
POST /api/auth/register
Body: {
  "email": "user@example.com",
  "password": "password123",
  "firstName": "John",
  "lastName": "Doe"
}
```

**Login**
```
POST /api/auth/login
Body: {
  "email": "user@example.com",
  "password": "password123"
}
```

Response:
```json
{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "user": {
    "id": 1,
    "email": "user@example.com",
    "role": "employee"
  }
}
```

### Tasks

**Create Task**
```
POST /api/tasks
Headers: Authorization: Bearer <token>
Body: {
  "title": "Task Title",
  "description": "Task Description",
  "priority": "high",
  "dueDate": "2024-12-31",
  "projectId": 1,
  "assigneeId": 2
}
```

**Get All Tasks**
```
GET /api/tasks
Headers: Authorization: Bearer <token>
```

**Get Task by ID**
```
GET /api/tasks/:id
Headers: Authorization: Bearer <token>
```

**Update Task**
```
PUT /api/tasks/:id
Headers: Authorization: Bearer <token>
Body: {
  "title": "Updated Title",
  "status": "in-progress"
}
```

**Delete Task**
```
DELETE /api/tasks/:id
Headers: Authorization: Bearer <token>
```

### Projects

**Create Project**
```
POST /api/projects
Headers: Authorization: Bearer <token>
Body: {
  "name": "Project Name",
  "description": "Project Description"
}
```

**Get All Projects**
```
GET /api/projects
Headers: Authorization: Bearer <token>
```

**Update Project**
```
PUT /api/projects/:id
Headers: Authorization: Bearer <token>
Body: {
  "name": "Updated Name",
  "description": "Updated Description"
}
```

**Delete Project**
```
DELETE /api/projects/:id
Headers: Authorization: Bearer <token>
```

## Role-Based Access Control

### Roles

**Admin**
- Full access to all features
- Manage users and roles
- View audit logs
- System configuration

**Project Manager**
- Create and manage projects
- Assign tasks to team members
- View project reports
- Cannot manage other users

**Employee**
- View assigned tasks
- Update task status
- View project information
- Limited to own tasks

## Audit Logging

All user actions are logged including:
- User login/logout
- Task creation, updates, deletions
- Project changes
- Role assignments
- Timestamp and user information

## Email Reminders

Automated cron jobs send email reminders for:
- Upcoming task deadlines
- Overdue tasks
- Task status updates

Configure the cron schedule in `.env` using the `CRON_EMAIL_REMINDER` variable.

## Project Structure

```
TaskFlow/
├── src/
│   ├── index.js                 # Application entry point
│   ├── config/
│   │   └── database.js         # Database connection config
│   ├── models/                 # Sequelize models
│   │   ├── User.js
│   │   ├── Task.js
│   │   ├── Project.js
│   │   └── AuditLog.js
│   ├── routes/                 # API routes
│   │   ├── auth.js
│   │   ├── tasks.js
│   │   ├── projects.js
│   │   └── users.js
│   ├── controllers/            # Request handlers
│   │   ├── authController.js
│   │   ├── taskController.js
│   │   ├── projectController.js
│   │   └── userController.js
│   ├── middleware/             # Express middleware
│   │   ├── auth.js            # JWT verification
│   │   └── auditLog.js        # Audit logging
│   └── utils/                  # Utility functions
│       └── logger.js
├── scripts/
│   └── seedDatabase.js         # Database seeder
├── .env.example               # Environment template
├── .gitignore                 # Git ignore rules
├── package.json               # Dependencies
├── Dockerfile                 # Docker setup
├── docker-compose.yml         # Docker compose
└── README.md                  # Documentation
```

## Testing Credentials

After running the seeder, use these credentials to test:

**Admin Account:**
- Email: admin@taskflow.com
- Password: Admin@123

**Project Manager Account:**
- Email: pm@taskflow.com
- Password: PM@123

**Employee Accounts:**
- Email: emp1@taskflow.com, Password: Emp@123
- Email: emp2@taskflow.com, Password: Emp@123

## Environment Variables

See `.env.example` for all available configuration options:
- `PORT` - Server port (default: 3000)
- `NODE_ENV` - Environment (development/production)
- `DB_*` - Database configuration
- `JWT_SECRET` - JWT signing secret (change in production)
- `JWT_EXPIRE` - Token expiration time
- `EMAIL_*` - Email service configuration

## Error Handling

The API returns standard HTTP status codes:
- 200 - OK
- 201 - Created
- 400 - Bad Request
- 401 - Unauthorized
- 403 - Forbidden
- 404 - Not Found
- 500 - Internal Server Error

## Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## License

This project is licensed under the MIT License - see the LICENSE file for details.

## Support

For support, email support@taskflow.com or open an issue on GitHub.
