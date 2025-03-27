# 🚀 Deploying openSIS with Docker


---

## 🧱 Step 1: Build the openSIS UI Docker Image

Make sure you're in the directory containing the `Dockerfile` for the openSIS UI:

```bash
docker build -t opensis-ui .
```

---

## 🌐 Step 2: Create a Docker Network

Create a custom Docker network so the containers can communicate:

```bash
docker network create opensis-net
```

---

## 🖥 Step 3: Run the openSIS UI Container

```bash
docker run --name opensis-ui --network opensis-net -p 8080:80 -d opensis-ui
```

---

## 🗄 Step 4: Run the MariaDB Database Container

```bash
docker run -d --name opensis-db \
  --restart always \
  -e MYSQL_ROOT_PASSWORD=abc123 \
  -e MYSQL_DATABASE=openSIS \
  -e MYSQL_USER=openSIS_rw \
  -e MYSQL_PASSWORD=Op3nS!S \
  -v db_data:/var/lib/mysql \
  -v $(pwd)/openSIS-Classis/MYSQL/mysql-init:/docker-entrypoint-initdb.d \
  -v $(pwd)/openSIS-Classis/MYSQL/mysql-config/strict_mode.cnf:/etc/mysql/conf.d/strict_mode.cnf \
  --network opensis_network \
  --network-alias opensis \
  mariadb:10.5
```

---

## 🛠 Step 5: Configure the MariaDB Database

Access the MariaDB shell:

```bash
docker exec -it opensis-db mariadb -u root -p
```

Enter the password (`root`), then run:

```sql
CREATE DATABASE openSIS DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
GRANT ALL PRIVILEGES ON *.* TO 'openSIS_rw'@'%' IDENTIFIED BY 'Op3nS!S' WITH GRANT OPTION;
FLUSH PRIVILEGES;
```

---

## ✅ Step 6: Access the openSIS Installation Page

Open your browser and go to:

```
http://localhost:8080/openSIS
```

---

