
<img src="https://github.com/user-attachments/assets/267c41db-b559-450e-bada-2f7a59aca6c8" width="400">

This project demonstrates a 2-tier web application architecture using Docker, where a Flask application and a MySQL database run in separate isolated containers. The application is built from a Git repository, connected using a custom Docker network, and uses persistent Docker volumes to ensure database data is not lost.

To optimize performance and reduce image size, a multi-stage Dockerfile is used. The final Docker images are pushed to Docker Hub for easy access and deployment. Performed advanced image analysis with Docker Scout to secure and optimize project Docker images.

## 🔹 Step 1: Clone the Git Repository
Git clone [Git Repository](https://github.com/LondheShubham153/two-tier-flask-app.git)

## 🔹 Step 2: Verify Database Configuration
```bash
MYSQL_HOST=localhost
MYSQL_USER=root
MYSQL_PASSWORD=your_password
MYSQL_DB=your_database 
```
## 🔹 Step 3: Create Dockerfile (Multi-Stage)
```bash
vim dockerfile
```

## 🔹 Step 4: Build Flask Application Image
```bash
docker build -t two-tier-app .
```

## 🔹 Step 5: Pull MySQL image
```bash
docker pull mysql:8.0
```

## 🔹 Step 6: Create Docker Network to connect both apps.
```bash
docker network create two-tier --driver bridge
```
verify:
```bash
docker network ls
```


## 🔹 Step 7: Create Docker Volume for MySQL Persistence
```bash
docker volume create mydata
```
check Volume:
```bash
docker volume ls
docker inspect mydata
```
<img width="400" height="400" alt="Screenshot 2026-02-03 at 10 31 21 PM" src="https://github.com/user-attachments/assets/6c0ffc4e-f398-4536-8708-834d20422d62" />

<img width="400" height="400" alt="Screenshot 2026-02-03 at 10 29 46 PM" src="https://github.com/user-attachments/assets/d9717b46-b0de-4ad2-bb78-cc71a2bb6c60" />



## 🔹 Step 8: Run MySQL Container (With Persistent Volume)

```bash
docker run -d \
  --name mysql \
  --network two-tier \
  -e MYSQL_ROOT_PASSWORD=root \
  -e MYSQL_DATABASE=devops \
  -v mydata:/var/lib/mysql \
  mysql:latest
```

## 🔹 Step 9: Run Flask Application Container
```bash
docker run -d \
  --name flask-app \
  --network two-tier \
  -p 5000:5000 \
  -e MYSQL_HOST=mysql \
  -e MYSQL_USER=root \
  -e MYSQL_PASSWORD=root \
  -e MYSQL_DB=devops \
  two-tier-app
```
## 🔹 Step 10: Verify Application

```bash
http://localhost:5000
```


<img width="600" height="600" alt="Screenshot 2026-02-03 at 2 35 26 PM" src="https://github.com/user-attachments/assets/49b666ec-b78b-4b4e-8545-0596fdb76cde" />

## 🔹 Step 11: Verify Database Data
```bash
docker exec -it mysql mysql -u root -p
```
<img width="500" height="450" alt="Screenshot 2026-02-02 at 2 13 47 PM" src="https://github.com/user-attachments/assets/8cd021b3-ace8-4c26-8e04-cd4d244efb30" />

ENTER:
```bash
:show databases
```
<img width="500" height="450" alt="Screenshot 2026-02-02 at 2 18 01 PM" src="https://github.com/user-attachments/assets/187d204d-f893-46f3-97f7-47824b0a1009" />


```bash
:use default_db
```
<img width="400" height="400" alt="Screenshot 2026-02-02 at 2 18 17 PM" src="https://github.com/user-attachments/assets/6252d979-1f02-492d-8184-a82b1f00ff13" />

```bash
:select * from messages
```
<img width="400" height="400" alt="Screenshot 2026-02-02 at 2 18 32 PM" src="https://github.com/user-attachments/assets/53ad541d-677e-44b5-9134-e61223b3b85b" />


## 🔹 Step 12: 📦 Docker Hub Image push

```bash
docker tag two-tier-app kruparajput/two-tier-app:latest
```
<img width="500" height="400" alt="Screenshot 2026-02-02 at 4 14 24 PM" src="https://github.com/user-attachments/assets/f341b308-5a43-4789-866c-d9b360c20027" />

PUSH IMAGE: 

```bash
docker push kruparajput/two-tier-app:latest
```
<img width="500" height="400" alt="Screenshot 2026-02-02 at 4 16 30 PM" src="https://github.com/user-attachments/assets/455a3ecf-2380-411f-b58c-2ddf626a8e6e" />

<img width="500" height="400" alt="Screenshot 2026-02-02 at 4 18 13 PM" src="https://github.com/user-attachments/assets/d8d1e7c3-8f78-4824-b8ae-2455d8a9e3c1" />

## 🔹 Step 13: Advanced Image analysis with Docker Scout

Analyze and inspect Docker images to ensure they are:

- Secure: Detects vulnerabilities in OS packages or dependencies

- Optimized: Highlights unnecessary layers, large files, and image bloat

- Compliant: Provides insights for license and security compliance

- Maintained: Checks if base images and dependencies are up-to-date

```bash
docker scout cves two-tier-app:latest
```
```bash
docker scout report two-tier-app:latest
```
<img width="1000" height="1000" alt="Screenshot 2026-02-06 at 2 22 21 AM" src="https://github.com/user-attachments/assets/b32448fa-da37-4926-baeb-ceb57e590146" />
