# 🚪 LosLibros - API Gateway

[![Java](https://img.shields.io/badge/Java-25-orange.svg)](https://openjdk.org/)
[![Spring Boot](https://img.shields.io/badge/Spring%20Boot-4.1.0-brightgreen.svg)](https://spring.io/projects/spring-boot)
[![Spring Cloud Gateway](https://img.shields.io/badge/Spring%20Cloud-Gateway%20(WebFlux)-blue.svg)](https://spring.io/projects/spring-cloud-gateway)

The **API Gateway** acts as the single unified entry point for all client and frontend traffic in the LosLibros Library Management System. Built using **Spring Cloud Gateway (Reactive WebFlux)**, it routes and load balances incoming HTTP requests to internal microservice instances registered in Eureka.

---

## 🌟 Features

- **Single Entry Point**: Consolidates multiple microservice endpoints into a unified API surface.
- **Dynamic Load Balancing**: Resolves downstream microservices dynamically via Eureka (`lb://<SERVICE_NAME>`).
- **Global CORS Handling**: Configured to handle Cross-Origin Resource Sharing for seamless Next.js frontend integration.
- **Header Preservation**: Includes `PreserveHostHeader` filters for maintaining host fidelity across downstream proxied requests.
- **Actuator & Health Checks**: Exposes health metrics and dynamic route definitions.

---

## 🏗️ Routing Rules

- **Service Name**: `api-gateway`
- **Port**: `7000`
- **Config Server Source**: `http://localhost:9000` (or `http://config.platform:9000`)

| Route ID | Path Predicate | Target URI | Description |
| :--- | :--- | :--- | :--- |
| `book-service` | `/api/v1/books/**` | `lb://BOOK-SERVICE` | Proxies book operations and cover images |
| `member-service` | `/api/v1/members/**` | `lb://MEMBER-SERVICE` | Proxies member CRUD operations |
| `borrowing-service` | `/api/v1/borrowings/**` | `lb://BORROWING-SERVICE` | Proxies borrowing, checkout, and return operations |

---

## 🚀 Running the API Gateway

> **Prerequisites**: Ensure both the **Config Server** (Port `9000`) and the **Service Registry** (Port `9001`) are up and running before starting the Gateway.

### Launch via Maven Wrapper

```bash
cd platform/api-gateway
./mvnw spring-boot:run
```

### Launch via Built JAR

```bash
./mvnw clean package -DskipTests
java -jar target/Api-Gateway-1.0.0.jar
```

---

## 🌐 Sample Verification via Gateway

Once the gateway and microservices are running, send test requests to port `7000`:

```bash
# Book Service through Gateway
curl http://localhost:7000/api/v1/books

# Member Service through Gateway
curl http://localhost:7000/api/v1/members

# Borrowing Service through Gateway
curl http://localhost:7000/api/v1/borrowings
```
