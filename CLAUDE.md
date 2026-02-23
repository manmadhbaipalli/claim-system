# Claim Process Platform - Project Standards

## Tech Stack
- **Frontend**: ReactJS 18+ with TypeScript
- **Backend**: Python 3.11+ with FastAPI
- **Database**: PostgreSQL (primary), Redis (caching/sessions)
- **Authentication**: JWT with role-based access control
- **Payment Processing**: Stripe Connect, ACH/Wire integration
- **State Management**: React Context API with useReducer
- **API Client**: Axios with interceptors
- **Validation**: Pydantic (backend), Yup (frontend)
- **Testing**: Pytest (backend), Jest + React Testing Library (frontend)
- **Documentation**: FastAPI auto-generated docs, JSDoc for complex functions

## Project Structure
```
/
├── backend/                    # FastAPI application
│   ├── app/
│   │   ├── api/               # API routes
│   │   │   ├── v1/            # API version 1
│   │   │   │   ├── policies/  # Policy endpoints
│   │   │   │   ├── claims/    # Claims endpoints
│   │   │   │   └── payments/  # Payment endpoints
│   │   ├── core/              # Core configuration
│   │   ├── db/                # Database models and migrations
│   │   ├── models/            # Pydantic models
│   │   ├── services/          # Business logic
│   │   └── utils/             # Utilities and helpers
│   ├── tests/                 # Backend tests
│   └── requirements.txt       # Python dependencies
├── frontend/                  # React application
│   ├── public/                # Static assets
│   ├── src/
│   │   ├── components/        # Reusable UI components
│   │   ├── pages/             # Page components
│   │   │   ├── policies/      # Policy management pages
│   │   │   ├── claims/        # Claims management pages
│   │   │   └── payments/      # Payment pages
│   │   ├── hooks/             # Custom React hooks
│   │   ├── services/          # API service functions
│   │   ├── types/             # TypeScript type definitions
│   │   ├── utils/             # Utility functions
│   │   └── contexts/          # React contexts for state
│   ├── tests/                 # Frontend tests
│   └── package.json           # Node.js dependencies
├── docs/                      # Project documentation
└── docker-compose.yml         # Development environment
```

## Coding Conventions

### Backend (Python)
- **Naming**: snake_case for variables/functions, PascalCase for classes
- **Imports**: Standard library → Third party → Local imports (separated by blank lines)
- **Type Hints**: Required for all function signatures and complex variables
- **Docstrings**: Google-style docstrings for all public functions/classes
- **Line Length**: 88 characters (Black formatter standard)
- **Error Handling**: Custom exceptions in `app/core/exceptions.py`

### Frontend (TypeScript/React)
- **Naming**: camelCase for variables/functions, PascalCase for components/types
- **Components**: Functional components with TypeScript interfaces for props
- **File Structure**: One component per file, index.ts for barrel exports
- **State Management**: Context + useReducer for complex state, useState for simple
- **API Calls**: All API interactions in `services/` directory
- **Styling**: CSS Modules or styled-components (consistent choice required)

### Security Standards
- **Data Masking**: SSN/TIN displayed as XXX-XX-XXXX format
- **PCI Compliance**: No payment data in logs, encrypted at rest and transit
- **Audit Logging**: All CRUD operations logged with user ID and timestamp
- **Access Control**: Role-based permissions enforced at API and UI levels

## Commands

### Backend
```bash
# Development server
cd backend && uvicorn app.main:app --reload --host 0.0.0.0 --port 8000

# Install dependencies
cd backend && pip install -r requirements.txt

# Run tests
cd backend && pytest tests/ -v

# Database migrations
cd backend && alembic upgrade head

# Linting
cd backend && flake8 app/ && black app/ --check
```

### Frontend
```bash
# Development server
cd frontend && npm start

# Install dependencies
cd frontend && npm install

# Run tests
cd frontend && npm test

# Build for production
cd frontend && npm run build

# Linting
cd frontend && npm run lint
```

### Full Stack
```bash
# Start all services (requires Docker)
docker-compose up -d

# Run all tests
./scripts/test-all.sh
```

## Key Patterns

### API Response Format
```python
{
    "data": {...},           # Response payload
    "message": "string",     # Human-readable message
    "success": boolean,      # Operation success flag
    "errors": [...]         # Validation/error details
}
```

### Error Handling
- Backend: Custom exceptions with HTTP status codes
- Frontend: Global error boundary with user-friendly messages
- Logging: Structured logging with correlation IDs

### Authentication Flow
- JWT tokens with refresh mechanism
- Role-based access control (Admin, Adjuster, Viewer)
- Protected routes on frontend, middleware on backend

### Data Validation
- Backend: Pydantic models for request/response validation
- Frontend: Yup schemas for form validation
- Database: SQLAlchemy models with constraints

### Audit Trail Pattern
```python
# All entities include audit fields
created_at: datetime
created_by: UUID
updated_at: datetime
updated_by: UUID
version: int           # For optimistic locking
```