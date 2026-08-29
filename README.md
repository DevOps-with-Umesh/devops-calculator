# DevOps Calculator

A simple Dockerized web application created to demonstrate an end-to-end **DevOps and DevSecOps CI/CD pipeline** using GitHub, Jenkins, SonarQube, Docker, Trivy, Docker Hub, and AWS EC2.

## 🚀 Application

**DevOps Calculator** is a simple web-based calculator that supports:

* Addition
* Subtraction
* Multiplication
* Division

The application is built using **HTML, CSS, and JavaScript** and served using **Nginx** inside a Docker container.

---

## 🏗️ Project Architecture

```text
Developer
    |
    v
  GitHub
    |
    v
 Jenkins
    |
    +--------------------+
    |                    |
    v                    v
SonarQube             Docker Build
Code Scan                  |
    |                      v
    v                    Trivy
Quality Gate           Image Scan
                           |
                           v
                      Docker Hub
                           |
                           v
                        AWS EC2
                           |
                           v
                    Docker Container
                           |
                           v
                         Nginx
                           |
                           v
                    DevOps Calculator
```

---

## 🛠️ Technologies Used

| Technology | Purpose                             |
| ---------- | ----------------------------------- |
| HTML       | Application UI                      |
| CSS        | Application styling                 |
| JavaScript | Calculator functionality            |
| Nginx      | Web server                          |
| Docker     | Containerization                    |
| Jenkins    | CI/CD automation                    |
| SonarQube  | Source code quality analysis        |
| Trivy      | Docker image vulnerability scanning |
| Docker Hub | Container image registry            |
| AWS EC2    | Application deployment              |
| GitHub     | Source code management              |

---

## 📁 Project Structure

```text
devops-calculator/
│
├── index.html
├── Dockerfile
├── Jenkinsfile
├── sonar-project.properties
└── README.md
```

### File Description

**index.html**

Contains the calculator application's HTML, CSS, and JavaScript.

**Dockerfile**

Creates the Docker image using Nginx.

**Jenkinsfile**

Defines the CI/CD pipeline.

**sonar-project.properties**

Contains SonarQube project configuration.

**README.md**

Project documentation.

---

## 🐳 Docker

### Build Docker Image

```bash
docker build -t devops-calculator .
```

### Run Docker Container

```bash
docker run -d \
  --name devops-calculator \
  -p 8080:80 \
  devops-calculator
```

### Check Running Container

```bash
docker ps
```

### Access Application

Open the following URL in your browser:

```text
http://localhost:8080
```

---

## 🔍 SonarQube

SonarQube is used to analyze the application source code and identify potential code quality issues.

The project configuration is defined in:

```text
sonar-project.properties
```

Example configuration:

```properties
sonar.projectKey=devops-calculator
sonar.projectName=DevOps Calculator
sonar.sources=.
sonar.exclusions=Dockerfile,Jenkinsfile,README.md,sonar-project.properties
```

The Jenkins pipeline performs the SonarQube scan before building the Docker image.

---

## 🔐 Trivy Security Scan

Trivy is used to scan the Docker image for vulnerabilities.

Example:

```bash
trivy image \
  --severity HIGH,CRITICAL \
  devops-calculator
```

The Jenkins pipeline fails if HIGH or CRITICAL vulnerabilities are detected.

---

## 🔄 CI/CD Pipeline

The Jenkins pipeline performs the following steps:

```text
1. Checkout Source Code
        ↓
2. SonarQube Code Scan
        ↓
3. SonarQube Quality Gate
        ↓
4. Build Docker Image
        ↓
5. Trivy Docker Image Scan
        ↓
6. Login to Docker Hub
        ↓
7. Push Docker Image
        ↓
8. Deploy Image to AWS EC2
        ↓
9. Verify Application
```

---

## 🐳 Docker Hub

After the security scan passes, Jenkins pushes the Docker image to Docker Hub.

Example:

```text
username/devops-calculator:latest
username/devops-calculator:BUILD_NUMBER
```

---

## ☁️ AWS EC2 Deployment

The Docker image is pulled from Docker Hub onto an AWS EC2 instance.

Example deployment:

```bash
docker pull username/devops-calculator:latest
```

Stop the existing container:

```bash
docker stop devops-calculator || true
```

Remove the existing container:

```bash
docker rm devops-calculator || true
```

Start the new container:

```bash
docker run -d \
  --name devops-calculator \
  --restart unless-stopped \
  -p 8080:80 \
  username/devops-calculator:latest
```

The application can then be accessed using:

```text
http://EC2-PUBLIC-IP:8080
```

---

## 🔑 Jenkins Credentials

The Jenkins pipeline requires the following credentials:

### Docker Hub

```text
Credential ID:
dockerhub-credentials
```

Used for pushing Docker images to Docker Hub.

### AWS EC2 SSH

```text
Credential ID:
ec2-ssh-key
```

Used by Jenkins to connect to the EC2 instance.

The private SSH key should **never be committed to GitHub**.

---

## ⚙️ Jenkins Requirements

The Jenkins server/agent should have:

* Git
* Docker
* SonarQube Scanner
* Trivy
* SSH client
* Jenkins SonarQube integration
* Jenkins SSH Agent plugin

---

## 🌐 EC2 Security Group

For testing the application from the internet, allow:

```text
SSH
TCP 22
Your IP / Jenkins IP

Custom TCP
TCP 8080
0.0.0.0/0
```

For production environments, restrict access instead of allowing `0.0.0.0/0`.

---

## 🎯 DevOps Concepts Demonstrated

This project demonstrates:

* Git-based source code management
* Jenkins Declarative Pipeline
* CI/CD automation
* Docker containerization
* Docker image management
* SonarQube code quality analysis
* Quality Gate
* Trivy vulnerability scanning
* Docker Hub image registry
* AWS EC2 deployment
* SSH-based deployment
* Automated application verification

---

## 👨‍💻 Author

**DevOpsWithUmesh**

DevOps Engineer | AWS | Docker | Kubernetes | Jenkins | Terraform | CI/CD

---

## 📌 Future Improvements

Possible future improvements:

* Deploy the application to Kubernetes/EKS
* Add Helm deployment
* Add Prometheus and Grafana monitoring
* Add HTTPS using a load balancer
* Implement blue-green deployment
* Implement rolling deployments
* Add automated rollback
* Add Terraform infrastructure provisioning
