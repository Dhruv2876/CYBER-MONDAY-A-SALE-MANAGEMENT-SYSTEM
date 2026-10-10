# Cyber Monday A SALE MANAGEMENT SYSTEM

A responsive sale management site with an HTML/CSS/JavaScript interface and a Java Servlet/JDBC backend project. The interface supports product listing, product creation, stock status, order creation, a Cyber Monday discount, and order summaries.

## Choose how to run it

### Front-end demonstration

Open `index.html` in a browser. This standalone demo stores sample products and orders in that browser's `localStorage`; it does not use a database.

### Java web application

The Maven web project is in `java-backend/`. Its JSP dashboard calls the Servlet API; product, promotion, and order data are stored in MySQL. The backend includes product/promotion/order repositories, OOP pricing and product models, transactional stock handling, and an asynchronous report job.

#### Prerequisites

- Java 17 or newer
- Maven 3.9 or newer
- MySQL 8
- Apache Tomcat 10.1

#### Setup and run

1. Create a MySQL user/database with permission to create tables and insert/update rows.
2. Run `java-backend/src/main/resources/db/schema.sql` against MySQL.
3. Set the database environment variables before starting Tomcat:
   - `SALE_DB_URL` (optional; default is `jdbc:mysql://localhost:3306/sale_desk?useSSL=false&serverTimezone=UTC`)
   - `SALE_DB_USER` (defaults to `root`)
   - `SALE_DB_PASSWORD` (required; no password is stored in the repository)
4. From `java-backend/`, run `mvn clean package`.
5. Copy `target/sale-desk.war` into Tomcat's `webapps` folder and start Tomcat.
6. Open `http://localhost:8080/sale-desk/`.

On Windows PowerShell, set the database connection values in the same terminal before starting Tomcat:

```powershell
$env:SALE_DB_URL = 'jdbc:mysql://localhost:3306/sale_desk?useSSL=false&serverTimezone=UTC'
$env:SALE_DB_USER = 'root'
$env:SALE_DB_PASSWORD = 'your-local-password'
```

#### API routes

- `GET /api/products` — list active products
- `POST /api/products` — create a product
- `GET /api/promotions` — list promotions
- `GET /api/promotions/active` — return the promotion active now
- `POST /api/promotions` — create a percentage promotion with start and end times
- `GET /api/orders` — list recent orders
- `POST /api/orders` — create an order; locks the product row and updates stock in one transaction
- `GET /api/metrics` — summary counts and recent-order revenue
- `POST /api/reports` — queue a background report job
- `GET /api/reports/{jobId}` — check a report job

## Rubric evidence in Java

| Marking area | Evidence in this project |
|---|---|
| GUI OOP implementation (10) | `Product` → `PhysicalProduct` / `DigitalProduct`; `Promotion` strategy interface; `PercentagePromotion`, `PromotionRule`, and `NoPromotion`; application exceptions mapped in `SaleApiServlet` |
| GUI Collections & Generics (6) | Typed `List<Product>`, `List<OrderReceipt>`, `Map<String, Future<...>>`, and typed repository return values |
| GUI Multithreading & Synchronization (4) | `ReportJobService` executor and synchronized job submission; `SaleService` per-product locks; MySQL row locks for concurrent stock updates |
| GUI database-operation classes (7) | `JdbcProductRepository`, `JdbcPromotionRepository`, and `JdbcOrderRepository` implement repository interfaces |
| GUI Database Connectivity (3) | `Database` provides JDBC connections from environment-based configuration |
| GUI JDBC implementation (3) | Prepared statements, generated keys, order transactions, commit/rollback, and try-with-resources |
| Web Problem Understanding & Solution Design (8) | This README documents the problem, workflows, architecture, schema, and run/deployment steps |
| Web Core Java Concepts (10) | Domain inheritance, pricing polymorphism, interfaces, generic collections, service classes, exceptions, and synchronization |
| Web Database Integration (JDBC) (8) | MySQL schema and JDBC repositories for products, promotions, and orders |
| Web Servlets & Web Integration (7) | `SaleApiServlet` exposes JSON endpoints; `index.jsp` serves the dashboard |

.
