# Interview Environment Setup

Environment สำหรับ Interview ที่พร้อมใช้งานทันที พร้อม PostgreSQL database และ Go starter project

## Prerequisites

- Go version ≥ 1.21
- Docker & Docker Compose
- Git (optional)

## Quick Start

### 1. Start PostgreSQL Database

```bash
docker compose up -d
```

ตรวจสอบว่า database ทำงาน:

```bash
docker compose ps
```

### 2. Verify Database Connection (Optional)

```bash
docker compose exec db psql -U postgres -d interview -c "SELECT version();"
```

### 3. Run Go Application

```bash
go run main.go
```

Server จะรันที่ `http://localhost:8080`

### 4. Test API

```bash
# Health check
curl http://localhost:8080/health

# Products endpoint (TODO: implement)
curl http://localhost:8080/products
```

## Project Structure

```
interview-tee/
├── docker-compose.yml    # PostgreSQL database configuration
├── go.mod                # Go module file
├── main.go              # Main application entry point
├── README.md            # This file
├── .gitignore           # Git ignore rules
└── internal/            # Internal packages (handlers, services, models, etc.)
```

## Database Configuration

- **Host**: localhost
- **Port**: 5432
- **User**: postgres
- **Password**: postgres
- **Database**: interview

### Connection String

```
postgres://postgres:postgres@localhost:5432/interview?sslmode=disable
```

## Development Tips

### Stop Database

```bash
docker compose down
```

### Stop Database and Remove Data

```bash
docker compose down -v
```

### View Database Logs

```bash
docker compose logs db
```

### Connect to Database via psql

```bash
docker compose exec db psql -U postgres -d interview
```

## TODO: Implement API Endpoints

ใน `main.go` มี TODO comments สำหรับ endpoints ที่ควร implement:

- `GET /products` - List all products
- `GET /products/:id` - Get product by ID
- `POST /products` - Create new product
- `PUT /products/:id` - Update product
- `DELETE /products/:id` - Delete product

## Testing Checklist

ก่อนเริ่ม Interview ควรทดสอบ:

- [ ] `docker compose up -d` - Database starts successfully
- [ ] `go run main.go` - Go application starts without errors
- [ ] `curl http://localhost:8080/health` - Health check returns OK
- [ ] Database connection works (test with your database driver)

## Troubleshooting

### Port 5432 already in use

```bash
# Check what's using the port
lsof -i :5432

# Or change port in docker-compose.yml
ports:
  - "5433:5432"  # Use 5433 instead
```

### Go module issues

```bash
go mod tidy
go mod download
```

### Docker issues

```bash
# Restart Docker service
# macOS: Restart Docker Desktop
# Linux: sudo systemctl restart docker
```

## Notes

- Database data จะถูกเก็บใน Docker volume `postgres_data`
- Server มี graceful shutdown handling
- Health check endpoint ใช้สำหรับตรวจสอบว่า server ทำงานอยู่
# interview-tee
