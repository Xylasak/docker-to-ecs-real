# Docker → ECS (EC2) Real Project

End-to-end containerization and deployment of a simple API using Docker and Amazon ECS with **EC2 launch type** (no Fargate).

This project demonstrates a **real production-style workflow**:
local development → containerization → image registry → ECS service running on EC2 instances.

## 🚀 Architecture Overview
```md
Local Machine (Docker)
↓
Docker Image (linux/amd64)
↓
Amazon ECR (Private Registry)
↓
Amazon ECS Service (EC2 launch type)
↓
Public EC2 Instance (port 8000)
```

## 🧱 Tech Stack

- **Backend**: Python (FastAPI)
- **Containerization**: Docker, Docker Compose
- **Registry**: Amazon ECR
- **Orchestration**: Amazon ECS (EC2 launch type)
- **Infrastructure**: EC2 (t3.micro, Amazon Linux 2)
- **Logging**: Amazon CloudWatch Logs

## Local Development

### Build image
```bash
docker build -t docker-to-ecs-real:dev .
docker compose up --build
curl http://localhost:8000
```
### Expected output
```json
{
  "service": "docker-to-ecs-real",
  "version": "compose-local"
}
```
## Cloud Deployment (ECS on EC2)

### Build and push image for ECS (amd64)
```bash
docker buildx build \
  --platform linux/amd64 \
  -t <ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com/docker-to-ecs-real:dev \
  --push \
  .
```

### ECS Service

* Launch type: EC2
* Desired tasks: 1
* Public access via EC2 public IPv4
* No load balancer (direct access)

## 🌐 Live Test

Once deployed, the API is reachable at:
```cpp
http://<EC2_PUBLIC_IP>:8000
```

## 🧪 Lessons Learned

* Docker images must match the target CPU architecture
* ECS failures are often image-related, not infrastructure-related
* Avoid using the latest tag in production
* ECS deployment rollbacks are a safety feature, not a failure
* Building and pushing images the same way CI/CD pipelines do (buildx --push)

## 📸 Screenshots

See the /images directory for:

* Amazon ECR repository with pushed image
* ECS cluster with EC2 instances
* ECS service with running task
* Application running in the browser


## 👤 Author

**Saliou**  
Cloud Engineer 
 
**Focus:**  
AWS • Terraform • Docker • ECS • CI/CD