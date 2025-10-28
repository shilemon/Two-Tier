# Two-Tier Flask + MySQL Application

This project demonstrates a **two-tier architecture** using **Flask** (web tier) and **MySQL** (database tier) with Docker. It includes:
- A Flask web application that stores and displays messages.
- A MySQL database for persistent storage.
- Docker and Docker Compose configurations for easy deployment.

---

## ✅ Features
- Flask app with MySQL integration using `Flask-MySQLdb`.
- Dockerized setup for both tiers.
- Optional Docker Compose for simplified orchestration.
- Predefined SQL schema (`message.sql`) for initializing the database.

---

## ✅ Prerequisites
- [Docker](https://docs.docker.com/get-docker/)
- [Docker Compose](https://docs.docker.com/compose/install/) (optional but recommended)

---

## ✅ Project Structure
```
Two-Tier/
├── app.py                # Flask application
├── requirements.txt      # Python dependencies
├── Dockerfile            # Flask app image
├── docker-compose.yml    # Compose configuration
├── message.sql           # SQL schema for messages table
└── templates/
    └── index.html        # HTML template
```

---

## ✅ Environment Variables
| Variable         | Description                  | Default   |
|-------------------|------------------------------|-----------|
| MYSQL_ROOT_PASSWORD | Root password for MySQL    | admin     |
| MYSQL_DATABASE    | Database name               | mydb      |
| MYSQL_USER        | App user for MySQL          | appuser   |
| MYSQL_PASSWORD    | Password for app user       | appsecret |

---

## ✅ Setup Instructions

### **Option 1: Using Docker Compose (Recommended)**
```bash
git clone https://github.com/shilemon/Two-Tier.git
cd Two-Tier
docker compose up --build
```

Once containers are up:
```bash
docker exec -i two-tier-db-1 mysql -uroot -padmin mydb < message.sql
```
Access the app at: [http://localhost:5000](http://localhost:5000)

---

### **Option 2: Using Docker CLI**
```bash
docker build -t flaskapp:latest .
docker network create twotier

# Start MySQL
docker run -d --name mysql --network=twotier   -e MYSQL_ROOT_PASSWORD=admin   -e MYSQL_DATABASE=mydb   -e MYSQL_USER=appuser   -e MYSQL_PASSWORD=appsecret   -p 33060:3306   mysql:5.7

# Initialize DB
docker exec -i mysql mysql -uroot -padmin mydb < message.sql

# Start Flask
docker run -d --name flaskapp --network=twotier   -e MYSQL_HOST=mysql   -e MYSQL_PORT=3306   -e MYSQL_USER=appuser   -e MYSQL_PASSWORD=appsecret   -e MYSQL_DB=mydb   -p 5000:5000   flaskapp:latest
```
Access the app at: [http://localhost:5000](http://localhost:5000)

---

## ✅ Troubleshooting
- **Port already allocated**: Use a different host port (e.g., `33060:3306`).
- **Unknown host 'mysql'**: Ensure both containers are on the same network (`twotier`).
- **Table doesn't exist**: Apply `message.sql` or enable `init_db()` in `app.py`.
- **Access denied**: Use the correct user (`appuser`) and password (`appsecret`).

---

## ✅ License
MIT
