# User Management Microservice

## Description
This is a small **enterprise-style Spring Boot application** that manages users, persists data in MySQL, and publishes events to Kafka. The application is **Dockerized** and can be built and deployed using **Jenkins**.

---

## Prerequisites
- Java JDK 17+
- Maven
- Docker Desktop
- Jenkins
- MySQL 8 (or via Docker Compose)
- Kafka + Zookeeper (via Docker Compose)
- Git
- Postman (for testing APIs)

---

## Setup

### 1. Start MySQL + Kafka via Docker Compose
```bash
docker-compose up -d

8d1f417851dd43c9aeb48af7305abac8


cd C:\Program Files\Jenkins

C:\Program Files\Jenkins>java -jar jenkins.war

ravi3_3e8ym6i


Clean old containers/networks:

FOR /F "tokens=*" %i IN ('docker ps -aq') DO docker stop %i
FOR /F "tokens=*" %i IN ('docker ps -aq') DO docker rm %i
docker network prune -f


Start Docker Compose:

docker-compose down --volumes

docker-compose up -d


--network user-management

endpoint I have set is port 8081 in localhost

http://localhost:8081/your-api

docker run -d --name usermgmt -p 8081:8080 --network user-management --link userdb:db --link kafka:kafka harshitshankar1998/usermgmt-app:latest
