# Practical 1: Git Basics (Windows)

## Part A – Setting Up Git

Install Git (via winget or chocolatey):

```powershell
winget install --id Git.Git -e --source winget
```

Verify installation:

```powershell
git --version
```

Check JDK version (17 or higher required):

```powershell
java -version
```

Configure Git:

```powershell
git config --global user.name "Your Name"
git config --global user.email "your-email@example.com"
git config --list
```

## Part B – Creating Git Repository

```powershell
mkdir git-practical
cd git-practical
git init
type nul > file1.txt
type nul > file2.txt
notepad file1.txt
notepad file2.txt
```

## Part C – Stage and Commit Files

```powershell
git status
git add .
git commit -m "Initial commit"
```

## Part D – Create and Connect GitHub Repository

```powershell
git remote add origin https://github.com/your-username/Aditi-DevOps-Batch-1.git
git branch -M main
git push -u origin main
```

## Part E – Basic Commands

```powershell
git status
git log
git pull origin main
```

## Part F – Clone Repository

```powershell
cd ..
git clone https://github.com/your-username/Aditi-DevOps-Batch-1.git
cd Aditi-DevOps-Batch-1
```

## Part G – Branching and Merging

```powershell
git checkout -b feature-branch
notepad file1.txt
git add .
git commit -m "Updated file in feature branch"
git checkout main
git merge feature-branch
git push origin main
```
# Practical 2: Jenkins CI/CD Setup (Windows)

## Part A – Verification of JDK and Git

```powershell
java -version
git --version
```

## Part B – Jenkins Installation

* Download Jenkins LTS (Windows installer) from Jenkins official website
* Install using default path
* Select Run service as Local System
* Set port to 8082 and test port
* Provide JDK installation path
* Keep default options and install

## Part C – Unlocking and Plugins

* Open [http://localhost:8082](http://localhost:8082)
* Copy initial admin password from:
  C:\Program Files\Jenkins\secrets\initialAdminPassword
* Install suggested plugins
* Create admin user
* Save Jenkins URL configuration

## Part D – Creating Repository (Git Bash)

```bash
cd Desktop
mkdir Aditi-DevOps-Project
cd Aditi-DevOps-Project
git init
touch app.py
git add .
git commit -m "Initial commit"
```

## Part E – Blue Ocean Setup

* Go to Manage Jenkins → Plugins
* Search Blue Ocean
* Install plugin
* Open Blue Ocean from menu

## Part F – Create Pipeline (Blue Ocean)

* Create New Pipeline
* Select Git
* Use local repo path:
  file:///C:/Users/YourUsername/Desktop/Aditi-DevOps-Project
* Create pipeline
* Select any agent

## Part G – Pipeline Execution

* Add Stage: Build
  Command:

```bash
echo "Compiling Application..."
```

* Run pipeline

* Add Stage: Test

* Add Stage: Deploy

* Add Stage: End

* Run pipeline after each addition

## Part H – Nested Stages

* Add sub-stages inside Build:

  * Build Stage
  * Archive Artifacts
* Run pipeline

## Part I – Handling Failures

* Modify test stage:

```bash
echo Running critical test... && exit 1
```

* Run pipeline to observe failure

## Part J – Advanced Pipeline (Jenkinsfile)

Create Jenkinsfile in project:

```groovy
pipeline {
    agent any
    stages {
        stage('Initialize') {
            steps {
                bat '''
                echo Building branch: %BRANCH_NAME%
                echo Build Number: %BUILD_NUMBER%
                '''
            }
        }
        stage('Build') {
            steps {
                bat 'echo "Compiling"'
                bat 'echo "Build Version 1.0" > build-info.txt'
                archiveArtifacts artifacts: 'build-info.txt', allowEmptyArchive: true
            }
        }
        stage('Test') {
            steps {
                bat 'echo "Tests Passed"'
            }
        }
        stage('Deploy') {
            steps {
                bat 'echo "Deploying application"'
            }
        }
        stage('End') {
            steps {
                bat 'echo "Pipeline completed successfully"'
            }
        }
    }
}
```

Run pipeline after committing Jenkinsfile:

```bash
git add Jenkinsfile
git commit -m "Added Jenkinsfile"
git push
```

# Practical 3: Docker Setup (Windows)

## Part A – Installation and Setup

Install Docker Desktop:
[https://docs.docker.com/desktop/setup/install/windows-install/](https://docs.docker.com/desktop/setup/install/windows-install/)

Ensure WSL is installed and updated:

```powershell
wsl --install
wsl --update
```

Open Docker Desktop and complete setup

Verify installation:

```powershell
docker --version
docker info
```

## Part B – Image Creation and Container Execution

Run test container:

```powershell
docker run hello-world
```

Create project directory:

```powershell
mkdir docker-project
cd docker-project
```

Create Dockerfile:

```powershell
notepad Dockerfile
```

Example Dockerfile:

```dockerfile
FROM node:18
WORKDIR /app
COPY . .
CMD ["node", "app.js"]
```

Build image:

```powershell
docker build -t my-app .
```

Run container:
# Practical 6: Docker Compose (Windows)

## Part A – Preparation and Setup

Create project directory:

```powershell
mkdir docker-compose-project
cd docker-compose-project
```

## Part B – Application and Container Configuration

Create docker-compose.yml:

```powershell
notepad docker-compose.yml
```

Example docker-compose.yml:

```yaml
version: '3'
services:
  web:
    image: nginx
    ports:
      - "8080:80"
```

Run Docker Compose:

```powershell
docker compose up -d
```

Verify containers:

```powershell
docker ps
```

Open browser:
[http://localhost:8080](http://localhost:8080)

Stop Docker Compose:

```powershell
docker compose down
```

```powershell
docker run -d -p 3000:3000 my-app
```


# Practical 9: Kubernetes Deployment (Windows + Minikube)

## Part A – Installation and Setup

Install kubectl:

```powershell
winget install -e --id Kubernetes.kubectl
```

Verify kubectl:

```powershell
kubectl version --client
```

Install Minikube:

```powershell
winget install -e --id Kubernetes.minikube
```

Verify Minikube:

```powershell
minikube version
```

## Part B – Minikube Cluster

Start cluster:

```powershell
minikube start
```

Verify cluster:

```powershell
kubectl get nodes
```

## Part C – Deploy Application

Create deployment:

```powershell
kubectl create deployment nginx-deployment --image=nginx
```

Check pods:

```powershell
kubectl get pods
```

Expose deployment:

```powershell
kubectl expose deployment nginx-deployment --type=NodePort --port=80
```

Access service:

```powershell
minikube service nginx-deployment
```

## Part D – Scaling Application

Scale deployment:

```powershell
kubectl scale deployment nginx-deployment --replicas=3
```

Verify scaling:

```powershell
kubectl get pods
```

## YAML for Scaling Deployment

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 0
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80
        resources:
          requests:
            memory: "64Mi"
            cpu: "250m"
          limits:
            memory: "128Mi"
            cpu: "500m"
```

# Practical 10: Kubernetes Scaling (Windows + Minikube)

## Part A – Installation and Setup

Ensure Docker Desktop is running and kubectl + Minikube are installed

Start Minikube:

```powershell
minikube start
```

Verify cluster:

```powershell
kubectl get nodes
```

## Part B – Deploy Application

Create deployment YAML:

```powershell
notepad deployment.yml
```

Example deployment:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx
        ports:
        - containerPort: 80
```

Apply deployment:

```powershell
kubectl apply -f deployment.yml
```

Verify:

```powershell
kubectl get deployments
kubectl get pods
```

## Part C – Scaling Application

Scale to 3 replicas:

```powershell
kubectl scale deployment nginx-deployment --replicas=3
```

Verify:

```powershell
kubectl get pods
```

Scale down to 1:

```powershell
kubectl scale deployment nginx-deployment --replicas=1
```

Self-healing (delete pod):

```powershell
kubectl delete pod <pod-name>
kubectl get pods
```

Expose service:

```powershell
kubectl expose deployment nginx-deployment --type=NodePort --port=80
```

## Part D – Access Application

```powershell
minikube service nginx-deployment
```

# Practical 11: Prometheus and Grafana (Windows)

## Part A – Prometheus Setup

Download Prometheus from official website

Extract and edit config:

```powershell
notepad prometheus.yml
```

Basic config:

```yaml
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: 'prometheus'
    static_configs:
      - targets: ['localhost:9090']
```

Start Prometheus:

```powershell
./prometheus.exe
```

Access:
[http://localhost:9090](http://localhost:9090)

## Part B – Windows Exporter

Download Windows Exporter from GitHub

Install and run service

Verify metrics:
[http://localhost:9182/metrics](http://localhost:9182/metrics)

Update prometheus.yml:

```yaml
  - job_name: 'windows'
    static_configs:
      - targets: ['localhost:9182']
```

Restart Prometheus and verify targets

## Part C – Grafana Setup

Download and install Grafana

Start Grafana and open:
[http://localhost:3000](http://localhost:3000)

Login:
admin / admin

Add data source:

* Type: Prometheus
* URL: [http://localhost:9090](http://localhost:9090)

Create dashboards:

* CPU Usage
* Memory Usage

---

# Practical 12: ELK Stack (WSL Ubuntu)

## Part A – Setup

```bash
wsl --list
wsl
sudo apt update
```

## Part B – Elasticsearch

```bash
sudo apt install openjdk-17-jdk -y
java -version
```

Download and install Elasticsearch

Start service:

```bash
sudo systemctl start elasticsearch
curl http://localhost:9200
```

Reset password:

```bash
sudo /usr/share/elasticsearch/bin/elasticsearch-reset-password -u elastic
```

## Part C – Logstash and Kibana

Install Logstash:

```bash
sudo apt install logstash -y
logstash --version
```

Install Kibana:

```bash
sudo apt install kibana -y
sudo systemctl start kibana
```

Access Kibana:
[http://localhost:5601](http://localhost:5601)

Login:

* Username: elastic
* Password: (generated earlier)

## Part D – Logs

Generate test log and send to Elasticsearch

Create Data View in Kibana

Go to Discover and view logs
