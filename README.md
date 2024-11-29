# 🎲 DevOps Ultimate Pipeline: Board Game Application Deployment

## 🌐 Project Overview
Comprehensive DevOps CI/CD pipeline demonstrating modern cloud-native application deployment and management.

## 🏗️ Architecture Diagram
```
[Git Repo] → [Jenkins] → [SonarQube] 
   ↓             ↓           ↓
[Compile] → [Test] → [Build] → [Docker] 
   ↓             ↓           ↓
[Nexus] → [Kubernetes] → [Monitoring]
```

## 🛠️ Technical Stack
- **Container Orchestration**: Kubernetes 1.28.1
- **CI/CD**: Jenkins
- **Containerization**: Docker
- **Code Quality**: SonarQube
- **Artifact Management**: Nexus
- **Monitoring**: Prometheus, Grafana
- **Security Scanning**: Trivy


## 🔧 Key Configurations

### Kubernetes Cluster Setup
```bash
# Initialize Kubernetes Master Node
sudo kubeadm init --pod-network-cidr=10.244.0.0/16

# Configure Kubernetes Cluster
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config

# Deploy Network Solution (Calico)
kubectl apply -f https://docs.projectcalico.org/manifests/calico.yaml
```

### Jenkins Pipeline Snippet
```groovy
pipeline {
    agent any
    stages {
        stage('Build & Scan') {
            steps {
                sh "mvn compile"
                sh "trivy fs --format table ."
                withSonarQubeEnv('sonar') {
                    sh "sonar-scanner -Dsonar.projectKey=BoardGame"
                }
            }
        }
        stage('Docker Build') {
            steps {
                script {
                    docker.build("boardgame:${env.BUILD_NUMBER}")
                }
            }
        }
        stage('Kubernetes Deploy') {
            steps {
                kubernetesDeploy(configs: "deployment.yaml")
            }
        }
    }
}
```

### Kubernetes Service Account
```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: jenkins
  namespace: webapps

---
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: app-role
  namespace: webapps
rules:
  - apiGroups: ["*"]
    resources: ["*"]
    verbs: ["get", "list", "watch", "create", "update", "patch", "delete"]
```

## 🚀 Pipeline Stages
1. Source Code Checkout
2. Compilation
3. Testing
4. Security Scanning
   - File System Scan
   - SonarQube Analysis
5. Build Artifact
6. Docker Image Creation
7. Kubernetes Deployment
8. Monitoring Setup

## 🔒 Security Implementations
- Static Code Analysis
- Container Image Scanning
- Kubernetes RBAC
- Service Account Authentication

## 📊 Monitoring Setup
```bash
# Prometheus Configuration
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: 'kubernetes-nodes'
    kubernetes_sd_configs:
      - role: node
```

## 📦 Prerequisites
- Kubernetes Cluster
- Jenkins
- Docker
- Maven
- SonarQube Scanner


# BoardgameListingWebApp

## Description

**Board Game Database Full-Stack Web Application.**
This web application displays lists of board games and their reviews. While anyone can view the board game lists and reviews, they are required to log in to add/ edit the board games and their reviews. The 'users' have the authority to add board games to the list and add reviews, and the 'managers' have the authority to edit/ delete the reviews on top of the authorities of users.  

## Technologies

- Java
- Spring Boot
- Amazon Web Services(AWS) EC2
- Thymeleaf
- Thymeleaf Fragments
- HTML5
- CSS
- JavaScript
- Spring MVC
- JDBC
- H2 Database Engine (In-memory)
- JUnit test framework
- Spring Security
- Twitter Bootstrap
- Maven

## Features

- Full-Stack Application
- UI components created with Thymeleaf and styled with Twitter Bootstrap
- Authentication and authorization using Spring Security
  - Authentication by allowing the users to authenticate with a username and password
  - Authorization by granting different permissions based on the roles (non-members, users, and managers)
- Different roles (non-members, users, and managers) with varying levels of permissions
  - Non-members only can see the boardgame lists and reviews
  - Users can add board games and write reviews
  - Managers can edit and delete the reviews
- Deployed the application on AWS EC2
- JUnit test framework for unit testing
- Spring MVC best practices to segregate views, controllers, and database packages
- JDBC for database connectivity and interaction
- CRUD (Create, Read, Update, Delete) operations for managing data in the database
- Schema.sql file to customize the schema and input initial data
- Thymeleaf Fragments to reduce redundancy of repeating HTML elements (head, footer, navigation)

## How to Run

1. Clone the repository
2. Open the project in your IDE of choice
3. Run the application
4. To use initial user data, use the following credentials.
  - username: bugs    |     password: bunny (user role)
  - username: daffy   |     password: duck  (manager role)
5. You can also sign-up as a new user and customize your role to play with the application! 😊
