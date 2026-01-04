# Multi-Container React Docker Application

A full-stack Fibonacci calculator application demonstrating microservices architecture with Docker containerization. The application consists of a React frontend, Express.js backend API, a background worker service, PostgreSQL database, Redis cache, and Nginx reverse proxy.

## Architecture

The application is built using a microservices architecture with the following components:

- **Client**: React frontend application
- **Server**: Express.js API server
- **Worker**: Background service for calculating Fibonacci numbers
- **PostgreSQL**: Database for storing indices
- **Redis**: Message broker (pub/sub) and cache for calculated values
- **Nginx**: Reverse proxy and load balancer

## How It Works

1. User enters a Fibonacci index in the React frontend
2. The request is routed through Nginx to the Express API server
3. The API server:
   - Stores the index in PostgreSQL
   - Publishes the index to Redis via pub/sub
   - Returns a response to the client
4. The worker service:
   - Subscribes to Redis pub/sub channel
   - Calculates the Fibonacci number for the received index
   - Stores the result back in Redis
5. The frontend periodically fetches calculated values from Redis and displays them

## Prerequisites

- Docker and Docker Compose installed on your system
- Git (for cloning the repository)

## Getting Started

### Development Mode

1. Clone the repository:
   ```bash
   git clone git@github.com:aqibh25/multi-react-docker.git
   cd multi-react-docker
   ```

2. Start the development environment:
   ```bash
   docker-compose -f docker-compose-dev.yml up
   ```

   This will:
   - Build and start all services with hot-reload enabled
   - Start PostgreSQL and Redis containers
   - Mount your local code as volumes for live updates
   - Expose the application on `http://localhost:3000`

3. Access the application:
   - Open your browser and navigate to `http://localhost:3000`
   - View calculated values as they become available

### Production Mode

1. Build the production images:
   ```bash
   docker-compose build
   ```

2. Set up environment variables:
   Create a `.env` file in the root directory with the following variables:
   ```
   REDIS_HOST=your_redis_host
   REDIS_PORT=6379
   PGUSER=your_postgres_user
   PGHOST=your_postgres_host
   PGDATABASE=your_database_name
   PGPASSWORD=your_postgres_password
   PGPORT=5432
   ```

3. Start the production environment:
   ```bash
   docker-compose up -d
   ```

   The application will be available on `http://localhost:80`


## Technologies Used

### Frontend
- React 18.2.0
- React Router DOM 4.3.1
- Axios 0.18.0

### Backend
- Express.js 4.16.3
- PostgreSQL (pg 8.0.3)
- Redis 2.8.0
- CORS 2.8.4
- Body Parser

### Infrastructure
- Docker & Docker Compose
- Nginx
- PostgreSQL 
- Redis 

## Development

### Making Changes

In development mode, changes to your code are automatically reflected:
- React app: Changes in `client/src/` will hot-reload
- Server: Changes in `server/` will restart the server (nodemon)
- Worker: Changes in `worker/` will restart the worker (nodemon)

### Stopping the Application

To stop all containers:
```bash
docker-compose -f docker-compose-dev.yml down
```

To stop and remove volumes (clears database data):
```bash
docker-compose -f docker-compose-dev.yml down -v
```
