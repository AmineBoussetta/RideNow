# RideNow - Microservices Ride-Sharing Platform

A microservices-based ride-sharing application built with FastAPI, demonstrating distributed system architecture with independent services for users, pricing, and ride management.

## 📋 Table of Contents

- [Overview](#overview)
- [Architecture](#architecture)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Running the Services](#running-the-services)
- [API Documentation](#api-documentation)
- [Project Structure](#project-structure)
- [Technologies Used](#technologies-used)
- [Future Services](#future-services)

## 🎯 Overview

RideNow is a microservices-based ride-sharing platform that separates concerns into independent services:
- **Users Service**: Manages drivers and their availability
- **Pricing Service**: Handles zone-based pricing rules
- **Payment Service**: (Planned) Payment processing
- **Ride Service**: (Planned) Ride booking and management

Each service operates independently with its own database and API endpoints, communicating via HTTP REST APIs.

## 🏗️ Architecture

```
RideNow/
├── users-service/      # Driver management (Port 8001)
├── pricing-service/    # Pricing rules (Port 8002)
├── payment-service/    # Payment processing (Planned)
└── ride-service/       # Ride booking (Planned)
```

### Service Communication
- Services communicate via REST APIs
- Each service has its own SQLite database
- Services can be scaled independently
- Health check endpoints available for monitoring

## 🔧 Prerequisites

- **Python 3.10+** (tested with Python 3.13.1)
- **pip** (Python package manager)
- **Virtual environment** (recommended)

## 📦 Installation

### 1. Clone the Repository

```bash
git clone <repository-url>
cd Tp3
```

### 2. Create and Activate Virtual Environment

**Windows:**
```bash
python -m venv .venv
.\.venv\Scripts\activate
```

**Mac/Linux:**
```bash
python3 -m venv .venv
source .venv/bin/activate
```

### 3. Install Dependencies

```bash
pip install fastapi uvicorn sqlalchemy pydantic
```

Or create a `requirements.txt` file:
```
fastapi>=0.104.0
uvicorn[standard]>=0.24.0
sqlalchemy>=2.0.0
pydantic>=2.0.0
```

Then install:
```bash
pip install -r requirements.txt
```

## 🚀 Running the Services

### Users Service (Port 8001)

Navigate to the users service directory and start the server:

```bash
cd ridenow/users-service
uvicorn users_service_app:app --reload --port 8001
```

**Swagger Documentation:** http://localhost:8001/docs

**Health Check:** http://localhost:8001/health

### Pricing Service (Port 8002)

Navigate to the pricing service directory and start the server:

```bash
cd ridenow/pricing-service
uvicorn pricing_service_app:app --reload --port 8002
```

**Swagger Documentation:** http://localhost:8002/docs

**Health Check:** http://localhost:8002/health

### Running Multiple Services

Open separate terminal windows/tabs for each service, or use a process manager like `pm2` or `supervisor`.

## 📚 API Documentation

### Users Service API

#### Health Check
- **GET** `/health`
  - Returns service status

#### Driver Management
- **POST** `/drivers`
  - Create a new driver
  - Request body: `{ "name": "string", "zone": "string", "available": boolean }`
  
- **GET** `/drivers`
  - List all drivers
  - Query parameters:
    - `available` (optional): Filter by availability (true/false)
    - `zone` (optional): Filter by zone
  
- **GET** `/drivers/{driver_id}`
  - Get driver by ID
  
- **PATCH** `/drivers/{driver_id}/availability`
  - Update driver availability
  - Request body: `{ "available": boolean }`

**Example:**
```bash
# Create a driver
curl -X POST "http://localhost:8001/drivers" \
  -H "Content-Type: application/json" \
  -d '{"name": "John Doe", "zone": "A", "available": true}'

# List available drivers in zone A
curl "http://localhost:8001/drivers?available=true&zone=A"
```

### Pricing Service API

#### Health Check
- **GET** `/health`
  - Returns service status

#### Pricing Management
- **POST** `/prices`
  - Create a pricing rule
  - Request body: `{ "from_zone": "string", "to_zone": "string", "amount": float }`
  
- **GET** `/price?from={zone}&to={zone}`
  - Get price for a route
  - Query parameters:
    - `from`: Origin zone (required)
    - `to`: Destination zone (required)

**Example:**
```bash
# Create a pricing rule
curl -X POST "http://localhost:8002/prices" \
  -H "Content-Type: application/json" \
  -d '{"from_zone": "A", "to_zone": "B", "amount": 12.50}'

# Get price for route A to B
curl "http://localhost:8002/price?from=A&to=B"
```

## 📁 Project Structure

```
Tp3/
├── README.md
├── docker-compose.yml          # Docker configuration (planned)
├── ridenow/
│   ├── users-service/
│   │   ├── users_service_app.py    # Users service application
│   │   └── users.db                 # SQLite database (auto-created)
│   ├── pricing-service/
│   │   ├── pricing_service_app.py  # Pricing service application
│   │   └── pricing.db              # SQLite database (auto-created)
│   ├── payment-service/            # (Planned)
│   └── ride-service/               # (Planned)
└── .venv/                          # Virtual environment
```

## 🛠️ Technologies Used

- **FastAPI**: Modern, fast web framework for building APIs
- **Uvicorn**: ASGI server for running FastAPI applications
- **SQLAlchemy**: SQL toolkit and ORM for database operations
- **Pydantic**: Data validation using Python type annotations
- **SQLite**: Lightweight database for each service

## 🔮 Future Services

### Payment Service
- Payment processing
- Transaction management
- Payment history

### Ride Service
- Ride booking
- Ride status management
- Integration with Users and Pricing services

## 🧪 Testing

### Manual Testing

1. **Start both services:**
   ```bash
   # Terminal 1
   cd ridenow/users-service
   uvicorn users_service_app:app --reload --port 8001
   
   # Terminal 2
   cd ridenow/pricing-service
   uvicorn pricing_service_app:app --reload --port 8002
   ```

2. **Test Users Service:**
   - Visit http://localhost:8001/docs for interactive API documentation
   - Create a driver: `POST /drivers`
   - List drivers: `GET /drivers`
   - Update availability: `PATCH /drivers/{id}/availability`

3. **Test Pricing Service:**
   - Visit http://localhost:8002/docs for interactive API documentation
   - Create pricing rule: `POST /prices`
   - Get price: `GET /price?from=A&to=B`

## 📝 Notes

- Databases are automatically created on first service startup
- Each service uses SQLite for simplicity (can be upgraded to PostgreSQL/MySQL)
- Services run independently and can be deployed separately
- The `--reload` flag enables auto-reload during development

## 🤝 Contributing

This is a learning project demonstrating microservices architecture. Feel free to extend it with:
- Additional services
- Service-to-service communication
- Docker containerization
- Database migrations
- Unit and integration tests
- API gateway
- Service discovery

## 📄 License

This project is for educational purposes.

---

**Built with ❤️ using FastAPI and Python**
