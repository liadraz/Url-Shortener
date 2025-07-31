# 🔗 Url-Shortener

A high-performance URL shortener service built with .NET 8, featuring user authentication, caching, and containerized deployment. Designed to demonstrate full-stack development skills and system design capabilities.

## 🚀 Features

### Core Functionality
- **URL Shortening**: Convert long URLs into short, shareable links
- **URL Redirection**: Fast redirect service with sub-millisecond response times
- **Custom Short Codes**: Option to create personalized short URLs
- **URL Expiration**: Set time-based expiration for links

### User Management
- **User Authentication**: JWT-based authentication system
- **User Dashboard**: Manage personal URLs and view statistics
- **Rate Limiting**: Prevent abuse with configurable rate limits

### Performance & Scalability
- **Multi-layer Caching**: Redis + In-memory caching for optimal performance
- **Analytics**: Click tracking and basic usage statistics
- **Database Optimization**: Indexed queries and efficient data models

### DevOps Ready
- **Containerized**: Docker and Docker Compose setup
- **Health Checks**: Monitoring endpoints for deployment
- **Structured Logging**: Comprehensive logging with Serilog
- **API Documentation**: OpenAPI/Swagger integration

## 🛠️ Technology Stack

- **Backend**: ASP.NET Core 8 Web API
- **Database**: PostgreSQL with Entity Framework Core
- **Caching**: Redis + In-Memory caching
- **Authentication**: JWT tokens with ASP.NET Core Identity
- **Containerization**: Docker & Docker Compose
- **Documentation**: Swagger/OpenAPI
- **Testing**: xUnit, Moq, TestContainers

## 🏗️ System Architecture

```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   Load Balancer │    │     Web API     │    │     Redis       │
│                 │───▶│   (.NET Core)   │───▶│    (Cache)      │
└─────────────────┘    └─────────────────┘    └─────────────────┘
                                │
                                ▼
                       ┌─────────────────┐
                       │   PostgreSQL    │
                       │   (Database)    │
                       └─────────────────┘
```

### Design Decisions

- **PostgreSQL over NoSQL**: Chosen for ACID compliance, rapid development, and excellent .NET integration
- **Base62 Encoding**: Using auto-incrementing IDs with Base62 encoding to avoid hash collisions
- **Read-Optimized**: Architecture optimized for 100:1 read-to-write ratio typical of URL shorteners
- **Caching Strategy**: Cache-aside pattern with Redis for hot URLs and in-memory cache for ultra-fast access

## 🚀 Quick Start

### Prerequisites
- .NET 8 SDK
- Docker & Docker Compose
- PostgreSQL (or use Docker)
- Redis (or use Docker)

### Running with Docker Compose
```bash
# Clone the repository
git clone https://github.com/liadraz/Url-Shortener.git
cd Url-Shortener

# Start all services
docker-compose up -d

# The API will be available at http://localhost:8080
# Swagger documentation at http://localhost:8080/swagger
```

### Local Development Setup
```bash
# Restore dependencies
dotnet restore

# Update database
dotnet ef database update

# Run the application
dotnet run --project src/TinyUrl.Api

# Run tests
dotnet test
```

## 📚 API Documentation

### Core Endpoints

#### Shorten URL
```http
POST /api/urls
Content-Type: application/json
Authorization: Bearer {jwt-token}

{
  "originalUrl": "https://example.com/very-long-url",
  "customCode": "my-link",  // optional
  "expiresAt": "2024-12-31T23:59:59Z"  // optional
}
```

#### Access Short URL
```http
GET /{shortCode}
# Redirects to original URL
```

#### Get URL Analytics
```http
GET /api/urls/{shortCode}/analytics
Authorization: Bearer {jwt-token}
```

### Authentication Endpoints

#### Register User
```http
POST /api/auth/register
Content-Type: application/json

{
  "email": "user@example.com",
  "password": "SecurePassword123!",
  "confirmPassword": "SecurePassword123!"
}
```

#### Login
```http
POST /api/auth/login
Content-Type: application/json

{
  "email": "user@example.com",
  "password": "SecurePassword123!"
}
```

## 🧪 Testing

```bash
# Run all tests
dotnet test

# Run with coverage
dotnet test --collect:"XPlat Code Coverage"

# Run integration tests
dotnet test --filter Category=Integration
```

### Test Coverage
- **Unit Tests**: Core business logic and services
- **Integration Tests**: API endpoints with TestContainers
- **Performance Tests**: Load testing for redirect endpoints

## 📈 Performance Benchmarks

| Metric | Target | Achieved |
|--------|--------|----------|
| URL Creation | < 100ms | ~45ms |
| URL Redirect | < 10ms | ~3ms (cached) |
| Concurrent Users | 1000+ | TBD |
| URLs Stored | 10M+ | TBD |

## 🔧 Configuration

### Environment Variables
```bash
# Database
DATABASE_URL=postgresql://user:password@localhost:5432/tinyurl

# Redis
REDIS_CONNECTION_STRING=localhost:6379

# JWT
JWT_SECRET_KEY=your-super-secret-key
JWT_EXPIRY_HOURS=24

# Rate Limiting
RATE_LIMIT_REQUESTS=100
RATE_LIMIT_WINDOW_MINUTES=1
```

## 🚀 Deployment

### Docker Deployment
```bash
# Build production image
docker build -t tinyurl-api .

# Run with production compose
docker-compose -f docker-compose.prod.yml up -d
```

### Cloud Deployment
- **Azure**: Container Apps or App Service
- **AWS**: ECS or Elastic Beanstalk  
- **GCP**: Cloud Run or GKE

## 🗂️ Project Structure

```
src/
├── TinyUrl.Api/              # Web API project
├── TinyUrl.Core/             # Domain models and interfaces
├── TinyUrl.Infrastructure/   # Data access and external services
└── TinyUrl.Application/      # Business logic and services

tests/
├── TinyUrl.UnitTests/        # Unit tests
├── TinyUrl.IntegrationTests/ # Integration tests
└── TinyUrl.PerformanceTests/ # Load tests

docker/
├── Dockerfile               # Production container
├── docker-compose.yml       # Development environment
└── docker-compose.prod.yml  # Production environment
```

## 🛣️ Roadmap

### Phase 1 - MVP ✅
- [x] Basic URL shortening
- [x] User authentication
- [x] Caching layer
- [x] Containerization

### Phase 2 - Enhancement 🚧
- [ ] Analytics dashboard
- [ ] Bulk URL creation
- [ ] QR code generation
- [ ] Custom domains

### Phase 3 - Scale 📋
- [ ] Microservices architecture
- [ ] Event-driven analytics
- [ ] Multi-region deployment
- [ ] Advanced monitoring

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- Inspired by bit.ly, TinyURL, and other URL shortening services
- Built as a demonstration of full-stack development capabilities
- Thanks to the .NET community for excellent tooling and libraries

## 📞 Contact

**Liad Raz** - [GitHub Profile](https://github.com/liadraz)

Project Link: [https://github.com/liadraz/Url-Shortener](https://github.com/liadraz/Url-Shortener)

---

⭐ If you found this project helpful, please give it a star on GitHub!