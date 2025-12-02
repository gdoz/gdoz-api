# 🌐 GDOZ Technology Solutions - API

> A minimal and secure backend API for anonymous visit tracking with engineering practices focused on **simplicity**, **security**, **maintainability**, and **clarity**.

# 🌍 1. Overview

This project integrates:

* A **secure PHP API** for anonymous page visit tracking
* A **MySQL database** for storing aggregated visit metrics
* A **CI/CD system** with GitHub Actions

---

# 🛡️ 2. High-Level Security Model

Security is a primary concern in this project. While the application is intentionally simple, the architecture follows several best practices to minimize risk and ensure reliable operation even in shared hosting environments.

### **Goals**

* Protect server resources from unauthorized requests
* Prevent credential exposure
* Avoid collecting or storing personal data
* Ensure database integrity
* Maintain a small, auditable attack surface
* Enable safe public hosting of the entire codebase

### 🔐 Authentication & API Access

* Visit-tracking endpoint protected by **token-based authentication**
* Token stored exclusively in **GitHub Secrets**
* `.env` generated dynamically during deployment
* Unauthorized requests rejected early

### 🗄 Data Minimization

No personal data is collected:

* No IP
* No User-Agent
* No geolocation
* No cookies

Stored fields only:

* Timestamp
* Page path (URL)
* Referrer

### 🧱 Input & Query Safety

* All inputs validated and sanitized
* Prepared statements only
* No dynamic SQL
* Restricted input sizes

### 🚫 Attack Surface Minimization

* A single, stateless backend endpoint
* No file uploads
* No session management
* No SSR, templating, or dynamic routing
* No admin interfaces

---

# ⚡ 3. Backend API Features (PHP 8.3)

* Stateless POST endpoint
* Token-based authorization
* Sanitized inputs
* PDO prepared statements
* `.env` based configuration
* No cookies, no sessions, no PII


---

# 🗃️ 4. Database Setup

```sql
CREATE TABLE visit_logs (
  id BIGINT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
  page_path VARCHAR(255) NOT NULL,
  referrer VARCHAR(512) NULL,
  created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
  INDEX idx_path (page_path),
  INDEX idx_created_at (created_at)
) ENGINE = InnoDB DEFAULT CHARSET = utf8mb4 COLLATE = utf8mb4_unicode_ci;
```

No personal data is stored.

---


# 💻 5. Local Development

Create `config/.env` file from `config/.env.example` and set the variables.

Use Docker for local MySQL:

```bash
docker run -d --name docs-db \
  -e MYSQL_ROOT_PASSWORD=root \
  -e MYSQL_DATABASE=dbname \
  -p 3306:3306 mysql:8
```

Run PHP backend:

```bash
php -S localhost:8000 -t backend
```

Open terminal and call the endpoint:

```sh
curl -X POST http://localhost:8000/api/visit.php \
     -H "Content-Type: application/json" 
     -H "X-Visit-Token: token"
     -d '{"page_path": "/", "referrer": ""}'
```

---


# 🚀 6. CI/CD (GitHub Actions)

### `deploy-ftps.yml`

* Generates `.env`
* Deploys backend folder
* Manual approval required
* Uses FTP-Deploy-Action

---

# 🏗 7. Server Requirements

* Linux + Apache
* PHP 8.3
* MySQL/MariaDB
* FTPS enabled


---

# 📄 8. License

This project is licensed under the MIT License — see the [LICENSE](LICENSE) file for details.

---

# 🎉 Final Notes

This project aims to balance:

* simplicity
* clean design
* secure engineering
* portability
* educational value
