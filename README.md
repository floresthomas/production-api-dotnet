# Gym Billing API

> Production-grade .NET 8 API for managing recurring gym memberships, 
> automated billing, retries and subscription lifecycle.

![Status](https://img.shields.io/badge/status-in_development-yellow)
![.NET](https://img.shields.io/badge/.NET-8.0-512BD4?logo=dotnet&logoColor=white)
![License](https://img.shields.io/badge/license-MIT-green)

---

## 📖 About

Gyms and subscription-based businesses face a recurring problem: charging 
clients automatically every month, handling failed payments, plan changes 
and cancellations — without losing money, double-charging, or relying on 
manual intervention.

This project is a backend API that solves that, built with production-grade 
practices: automated testing, CI/CD, structured logging and cloud deployment.

It is being developed as a portfolio project to demonstrate clean architecture, 
batch processing, idempotency and real-world domain modeling in .NET 8.

---

## ✨ Features

### ✅ MVP (current scope)
- Create and manage subscription plans (e.g. `Basic`, `Premium`)
- Subscribe clients to a plan
- Trigger a billing run manually via API endpoint
- Mock payment processing with realistic failure simulation
- Idempotent billing runs (running the same day twice does not charge twice)
- Track payment status per subscription (`succeeded`, `failed`, `pending`)
- REST API documented with Swagger / OpenAPI

### 🚧 Planned (next iterations)
- Scheduled daily billing job (Hangfire) — runs automatically, skips Sundays
- Retry strategy with exponential backoff for failed payments
- Plan changes (upgrade / downgrade with proration)
- Subscription cancellation with end-of-period logic
- Notifications layer (email + webhooks) for failed / successful payments
- Admin reporting endpoints (active, churned, revenue per period)
- Integration with real payment provider (Stripe / MercadoPago)

---

## 🛠️ Tech Stack

| Category | Technology |
|---|---|
| **Language** | C# 12 |
| **Framework** | .NET 8, ASP.NET Core |
| **Database** | SQL Server |
| **ORM / Data** | EF Core + Dapper |
| **Background jobs** | Hangfire *(planned)* |
| **Logging** | Serilog (structured logs) |
| **Testing** | xUnit · Moq · WebApplicationFactory · TestContainers |
| **CI/CD** | GitHub Actions |
| **Containerization** | Docker |
| **Cloud** | Azure App Service |
| **Architecture** | Clean Architecture · DDD · SOLID |

---

## 🏗️ Architecture

The project follows **Clean Architecture** with clear separation of concerns:

```
src/
├── GymBilling.Domain         # Entities, value objects, business rules
├── GymBilling.Application    # Use cases, interfaces, DTOs
├── GymBilling.Infrastructure # DB access, external services, payment gateway
└── GymBilling.Api            # Controllers, middleware, DI configuration
```

**Key principles:**
- The `Domain` layer has **zero dependencies** on infrastructure or frameworks.
- Business rules live in the domain (e.g. *"a cancelled subscription cannot be billed"*).
- Use cases orchestrate the flow without knowing about HTTP or SQL Server.
- Infrastructure adapts external concerns (database, payment gateway, notifications).

---

## 🚀 Getting Started

### Prerequisites
- [.NET 8 SDK](https://dotnet.microsoft.com/download)
- [Docker](https://www.docker.com/) (for SQL Server container)

### Run locally
```bash
# Clone the repository
git clone https://github.com/floresthomas/production-api-dotnet.git
cd production-api-dotnet

# Start SQL Server in Docker
docker compose up -d

# Apply migrations and run the API
dotnet ef database update --project src/GymBilling.Infrastructure
dotnet run --project src/GymBilling.Api
```

The API will be available at `https://localhost:5001`.
Swagger UI: `https://localhost:5001/swagger`

### Run tests
```bash
dotnet test
```

---

## 📋 Domain Overview

The core entities of the system:

- **Plan** — A subscription tier (name, price, billing cycle)
- **Subscriber** — A client subscribed to a plan
- **Subscription** — The relationship between a Subscriber and a Plan, with status and dates
- **Payment** — A billing attempt for a subscription, with result (`succeeded` / `failed`)
- **BillingRun** — A daily execution that processes all due subscriptions

**Subscription lifecycle:**
```
created → active → [past_due] → active
                 ↘ cancelled
```

---

## 🗺️ Roadmap

- [x] Project scaffolding
- [ ] Domain model: Plan, Subscriber, Subscription, Payment
- [ ] REST endpoints (CRUD for plans and subscriptions)
- [ ] Manual billing run with idempotency
- [ ] Mock payment service with random failure
- [ ] Unit tests for domain logic
- [ ] Integration tests with WebApplicationFactory + TestContainers
- [ ] GitHub Actions pipeline (build + test)
- [ ] Dockerize and deploy to Azure App Service
- [ ] Automated scheduler with Hangfire
- [ ] Retry strategy for failed payments
- [ ] Subscription cancellation and plan changes
- [ ] Real payment provider integration

---

## 👤 Author

**Thomas Flores** — Backend .NET Developer  
📍 Buenos Aires, Argentina  
🔗 [LinkedIn](https://linkedin.com/in/thomasflores0) · [GitHub](https://github.com/floresthomas)  
📧 thomyeze57@gmail.com

---

## 📝 License

MIT — see [LICENSE](LICENSE) for details.
