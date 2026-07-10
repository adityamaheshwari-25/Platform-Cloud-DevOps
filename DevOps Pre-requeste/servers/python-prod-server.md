# Production Flask Architecture

## Architecture

```text
                User's Browser
                      │
              https://example.com
                      │
                      ▼
                 Nginx (Port 443)
        ┌───────────────────────────────┐
        │ TLS/SSL                       │
        │ Reverse Proxy                 │
        │ Static Files                  │
        │ Load Balancing (optional)     │
        └───────────────────────────────┘
                      │
            http://127.0.0.1:8000
                      │
                      ▼
              Gunicorn (WSGI Server)
        ┌───────────────────────────────┐
        │ Worker Management             │
        │ HTTP Request Handling         │
        │ Process Management            │
        └───────────────────────────────┘
                      │
                      ▼
                Flask Application
        ┌───────────────────────────────┐
        │ Routes                        │
        │ Business Logic                │
        │ Authentication                │
        │ Validation                    │
        └───────────────────────────────┘
                      │
                      ▼
                PostgreSQL Database
```

---

## Components

### 🌐 Nginx
Nginx is the **web server** and **reverse proxy**.

**Responsibilities**
- Handles HTTPS (TLS/SSL termination)
- Reverse proxies requests to Gunicorn
- Serves static files (CSS, JS, Images)
- Load balances traffic (optional)

---

### 🦄 Gunicorn

Gunicorn (**Green Unicorn**) is a **production-grade WSGI HTTP server** for Python applications.

Instead of running:

```bash
python app.py
```

Production applications typically run:

```bash
gunicorn app:app
```

**Responsibilities**
- Starts worker processes
- Receives HTTP requests from Nginx
- Passes requests to Flask via WSGI
- Manages worker lifecycle
- Returns responses back to Nginx

Example:

```bash
gunicorn -w 3 app:app
```

- `-w 3` → Start 3 worker processes
- First `app` → Python module (`app.py`)
- Second `app` → Flask application object inside `app.py`

Example:

```python
# app.py

from flask import Flask

app = Flask(__name__)
```

---

### 🔄 WSGI

**WSGI (Web Server Gateway Interface)** is the standard interface between a Python web server and a Python web application.

```
Browser
   │
 HTTP
   │
Gunicorn
   │
 WSGI
   │
Flask
```

---

### 🐍 Flask

Flask contains the application logic.

**Responsibilities**
- Routes
- Business logic
- Authentication
- Validation
- API responses
- Database interaction

Example:

```python
@app.route("/")
def home():
    return "Hello World"
```

---

### 🐘 PostgreSQL

Stores the application's persistent data.

Examples:
- Users
- Orders
- Products
- Transactions

---

# Request Flow

```
Browser
   │
   ▼
Nginx
   │
   ▼
Gunicorn
   │
   ▼
Flask
   │
   ▼
PostgreSQL
   │
   ▼
Flask
   │
   ▼
Gunicorn
   │
   ▼
Nginx
   │
   ▼
Browser
```

---

# Why this Architecture?

| Component | Responsibility |
|-----------|----------------|
| **Nginx** | HTTPS, Reverse Proxy, Static Files, Load Balancer |
| **Gunicorn** | Production WSGI Server, Worker Management |
| **Flask** | Application Logic & APIs |
| **PostgreSQL** | Persistent Database |

### Benefits

- Better Performance
- Improved Security
- Easy Scalability
- Fault Tolerance (multiple workers)
- Clear Separation of Responsibilities