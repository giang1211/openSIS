# 🚀 Deploying openSIS with Docker


## 🌐 Step 1: Create a Docker Network

To allow containers to communicate securely, create a custom Docker network:

```bash
docker network create opensis_network
```

---

## 🔐 Step 2: Create an `.env` File

Create a `.env` file in the same directory as your `Dockerfile` to store environment variables securely:

```bash
touch .env
```

Add the following content:

```
# Database credentials
MYSQL_ROOT_PASSWORD=your-root-password
MYSQL_DATABASE=openSIS
MYSQL_USER=openSIS_rw
MYSQL_PASSWORD=Op3nS!S
```

---

## 🗄️ Step 3: Create Docker Volumes

To persist the database data, create a named volume:

```bash
docker volume create db_data
```

---

## 🧱 Step 4: Build the openSIS UI Docker Image

Run the following command to build the Docker image:

```bash
docker build -t opensis-ui .
```


---

## 🗄️ Step 5: Run the MariaDB Container

Run the MariaDB container and load the environment variables from the `.env` file:

```bash
docker run -d --name opensis-db \
  --restart always \
  --env-file .env \
  -v db_data:/var/lib/mysql \
  -v $(pwd)/openSIS-Classic/MYSQL/mysql-init:/docker-entrypoint-initdb.d \
  -v $(pwd)/openSIS-Classic/MYSQL/mysql-config/strict_mode.cnf:/etc/mysql/conf.d/strict_mode.cnf \
  --network opensis_network \
  --network-alias opensis \
  mariadb:10.5
```

---

## 🎨 Step 6: Run the openSIS UI Container

Run the openSIS UI container on the same network:

```bash
docker run -d --name opensis-ui \
  --network opensis_network \
  -p 8080:80 \
  opensis-ui
```

---

## 🧪 Step 7: Test the openSIS Application

Once both containers are running:
- Open your browser and go to:
```
http://localhost:8080
```
- Follow the installation steps in the openSIS UI.
- Connect to the database using:
    - **Host:** `opensis`
    - **Database:** `openSIS`
    - **User:** `openSIS_rw`
    - **Password:** `Op3nS!S`

---

## 🧼 Step 8: Cleanup (Optional)

To stop and remove the containers:

```bash
docker stop opensis-ui opensis-db
docker rm opensis-ui opensis-db
```

To remove the Docker network:

```bash
docker network rm opensis_network
```

To remove the volumes:

```bash
docker volume rm db_data
```

-----

