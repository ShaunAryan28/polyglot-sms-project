# 🚀 Polyglot Event-Driven SMS Microservice

Welcome to the Polyglot SMS project! This repository is a fully functional, event-driven distributed system built to demonstrate how different programming languages can communicate seamlessly using message brokers.

Whether you are a recruiter looking at my architecture skills, or a beginner looking to learn about microservices, this project is designed to be easy to run and understand.

---

# 🧠 What Does This System Do?

This system simulates a backend architecture for sending SMS messages. Instead of one massive application doing everything, it splits the work into specialized services:

1. **The Gateway (Java/Spring Boot)**  
   Receives the user's request to send a text message.

2. **The Bouncer (Redis)**  
   Instantly checks if the user's phone number is on a blocked list.

3. **The Conveyor Belt (Apache Kafka)**  
   If approved, the Java app throws the message onto a high-speed message queue.

4. **The Worker (Go)**  
   A separate application written in Go constantly watches the Kafka queue. When it sees a new message, it grabs it.

5. **The Vault (MongoDB)**  
   The Go application permanently saves the message record into a database for future tracking.

---

# 🏗️ System Architecture

```text
+-----------------------+
|  Java Spring Boot API |
|  SMS Gateway Service  |
+-----------+-----------+
            |
            | Validate Request
            v
+-----------------------+
|        Redis          |
|  Blocklist Checking   |
+-----------+-----------+
            |
            | Publish Event
            v
+-----------------------+
|     Apache Kafka      |
|     sms-events Topic  |
+-----------+-----------+
            |
            | Consume Event
            v
+-----------------------+
|      Go Service       |
| SMS Consumer Service  |
+-----------+-----------+
            |
            | Store Record
            v
+-----------------------+
|       MongoDB         |
|   SMS History Store   |
+-----------------------+
```

---

# 🛠️ Tech Stack

| Technology | Purpose |
|------------|---------|
| Java 17 & Spring Boot 3 | Primary API Gateway |
| Go 1.22+ | Event Consumer & History API |
| Apache Kafka | Message Broker |
| Redis | In-Memory Cache |
| MongoDB | NoSQL Database |
| Docker & Docker Compose | Containerized Infrastructure |

---

# 📂 Project Structure

```bash
polyglot-sms-project/
│
├── docker-compose.yml
│
├── java-sms-sender/
│   ├── src/
│   ├── pom.xml
│   └── Dockerfile
│
└── go-sms-store/
    ├── main.go
    ├── go.mod
    └── Dockerfile
```

---

# 💻 How to Run This Project Locally

This project is designed to run almost entirely through Docker so you don't need to install databases manually on your machine.

---

## ✅ Prerequisites

Make sure you have the following installed:

- [Docker Desktop](https://www.docker.com/products/docker-desktop/)
- [Java 17](https://adoptium.net/)
- Maven
- [Go](https://go.dev/dl/)

---

# 🚀 Step 1: Start Infrastructure Services

Open your terminal in the project root directory and run:

```bash
docker compose up -d
```

This command automatically downloads and starts:

- Kafka
- Zookeeper
- Redis
- MongoDB

inside isolated Docker containers.

---

# ☕ Step 2: Start the Java Service

Open a new terminal and run:

```bash
cd java-sms-sender
mvn spring-boot:run
```

The Java API server will start on:

```text
http://localhost:8080
```

---

# 🐹 Step 3: Start the Go Service

Open another terminal and run:

```bash
cd go-sms-store
go run main.go
```

The Go service will:

- Connect to MongoDB
- Subscribe to Kafka events
- Start running on:

```text
http://localhost:8081
```

---

# 🎯 Testing the System

Once everything is running, you can test the complete event-driven pipeline.

---

# 📤 1. Send an SMS

This request hits the Java API.

Java will:

- Validate the request
- Check Redis
- Publish the message to Kafka

### PowerShell

```powershell
Invoke-RestMethod `
  -Uri "http://localhost:8080/v1/sms/send" `
  -Method Post `
  -Headers @{"Content-Type"="application/json"} `
  -Body '{
    "userId": "user-007",
    "phoneNumber": "+1234567890",
    "message": "Hello from the polyglot microservice!"
  }'
```

### cURL

```bash
curl -X POST http://localhost:8080/v1/sms/send \
-H "Content-Type: application/json" \
-d '{
  "userId": "user-007",
  "phoneNumber": "+1234567890",
  "message": "Hello from the polyglot microservice!"
}'
```

✅ Check your Go terminal — you should instantly see logs showing the message was consumed and stored.

---

# 📥 2. View Message History

To verify the data successfully traveled through Kafka and was stored in MongoDB:

### PowerShell

```powershell
Invoke-RestMethod `
  -Uri "http://localhost:8081/v1/user/user-007/messages"
```

### cURL

```bash
curl http://localhost:8081/v1/user/user-007/messages
```

---

# 🧠 Key Concepts Demonstrated

✅ Microservices Architecture  
✅ Event-Driven Systems  
✅ Polyglot Backend Communication  
✅ Kafka Producer & Consumer  
✅ Redis Caching  
✅ MongoDB Integration  
✅ REST APIs  
✅ Dockerized Development  
✅ Distributed System Design  

---

# 🐳 Infrastructure Services

| Service | Port |
|---------|------|
| Java API | `8080` |
| Go API | `8081` |
| Kafka | `9092` |
| Redis | `6379` |
| MongoDB | `27017` |

---

# 🔧 Useful Docker Commands

## Stop All Services

```bash
docker compose down
```

## Rebuild Everything

```bash
docker compose up --build
```

## View Running Containers

```bash
docker ps
```

---

# 📈 Future Improvements

- Add JWT Authentication
- Add Retry & Dead Letter Queues
- Add Rate Limiting
- Add SMS Provider Integration (Twilio)
- Add Kubernetes Deployment
- Add Monitoring with Prometheus & Grafana
- Add CI/CD Pipeline
- Add Unit & Integration Tests

---

# 🤝 Contributing

Contributions are welcome!

## Steps

1. Fork the repository

2. Create a feature branch

```bash
git checkout -b feature/amazing-feature
```

3. Commit your changes

```bash
git commit -m "Add amazing feature"
```

4. Push your branch

```bash
git push origin feature/amazing-feature
```

5. Open a Pull Request

---

# 👨‍💻 Author

**Shaun Aryan**  
Student at IIT (BHU) Varanasi  
Backend & Distributed Systems Enthusiast

GitHub: https://github.com/ShaunAryan28

---

# ⭐ Support

If you found this project useful:

- ⭐ Star the repository
- 🍴 Fork it
- 🛠️ Build on top of it

---

# 📜 License

This project is licensed under the MIT License.
