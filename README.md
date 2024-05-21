# DevSecOps for OpenAI Chatbot UI Deployment
![chatbotuidevops](https://github.com/SiddharthUchil/ChatGPT-UI-On-EKS/assets/36127139/6fccdbb6-3e76-4250-96b1-f4f7c78f031a)

This project demonstrates a DevSecOps approach for deploying an OpenAI Chatbot UI application on an Amazon Elastic Kubernetes Service (EKS) cluster. The deployment process leverages Jenkins as the CI/CD tool, Docker for containerization, and Terraform for provisioning the EKS cluster.

## Key Features

- **Automated Deployment:** The entire deployment process is automated using Jenkins pipelines, ensuring efficient and reliable delivery of updates and enhancements.
- **Security Scanning:** The application is scanned for vulnerabilities using OWASP Dependency Check and Trivy, ensuring a secure deployment.
- **Quality Assurance:** SonarQube is integrated for code quality analysis and enforcement of quality gates.
- **Containerization:** The application is containerized using Docker, enabling consistent and reproducible deployments.
- **Infrastructure as Code:** Terraform is used to provision the EKS cluster, enabling infrastructure management through code.

## Prerequisites

Before you begin, make sure you have the following prerequisites:

- AWS Account
- Jenkins Server
- Docker
- Terraform
- Kubernetes CLI (kubectl)

## Deployment Steps

1. **Launch an AWS EC2 Instance:**
   - Launch an Ubuntu 22.04 T2 Large instance and configure the necessary security groups and IAM roles.

2. **Install Dependencies:**
   - Install Jenkins, Docker, Trivy, Terraform, and Kubernetes CLI on the EC2 instance.

3. **Configure Jenkins:**
   - Install required plugins (e.g., SonarQube Scanner, NodeJS, Docker, Kubernetes) and set up global tool configurations.

4. **Set up SonarQube:**
   - Configure SonarQube server and quality gates for code analysis.

5. **Create Jenkins Pipeline:**
   - Define the Jenkins pipeline stages for checkout, dependency installation, security scanning, Docker build and push, and Kubernetes deployment.

6. **Provision EKS Cluster:**
   - Use Terraform to provision the EKS cluster and node groups.

7. **Deploy to EKS:**
   - Apply the Kubernetes deployment manifest to deploy the Chatbot UI application on the EKS cluster.

## Usage

1. **Clone the repository:**

- git clone https://github.com/SiddharthUchil/ChatGPT-UI-On-EKS.git


2. **Navigate to the Eks-terraform directory:**
- Update the `backend.tf` file with your S3 bucket name.

3. **Create a Jenkins job:**
- Configure the pipeline script according to the provided instructions.

4. **Run the Jenkins job:**
- Initiate the deployment process.

5. **Access the deployed Chatbot UI application:**
- Use the LoadBalancer DNS or external IP.


# DevOps Setup Guide: Jenkins, Docker, Trivy, and More

This guide provides step-by-step instructions for setting up a DevOps environment with Jenkins, Docker, Trivy, Terraform, and other essential tools. Follow these steps to streamline your development and deployment processes.

# DevOps Setup Guide: Jenkins, Docker, Trivy, and More

This guide provides step-by-step instructions for setting up a DevOps environment with Jenkins, Docker, Trivy, Terraform, and other essential tools. Follow these steps to streamline your development and deployment processes.

## Table of Contents

- Install Jenkins
- Install Docker
- Set Up SonarQube (Optional)
- Install Trivy, Kubectl, and Terraform
- Install Required Jenkins Plugins
- Create a Jenkins Pipeline

---


## Install Jenkins

1. **Connect to your console** (e.g., SSH into your server).
2. Create a `jenkins.sh` script with the following content:

    ```bash
    #!/bin/bash
    sudo apt update -y
    wget -O - https://packages.adoptium.net/artifactory/api/gpg/key/public | tee /etc/apt/keyrings/adoptium.asc
    echo "deb [signed-by=/etc/apt/keyrings/adoptium.asc] https://packages.adoptium.net/artifactory/deb $(awk -F= '/^VERSION_CODENAME/{print$2}' /etc/os-release) main" | tee /etc/apt/sources.list.d/adoptium.list
    sudo apt update -y
    sudo apt install temurin-17-jdk -y
    /usr/bin/java --version
    curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee /usr/share/keyrings/jenkins-keyring.asc > /dev/null
    echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] https://pkg.jenkins.io/debian-stable binary/ | sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null
    sudo apt-get update -y
    sudo apt-get install jenkins -y
    sudo systemctl start jenkins
    ```

3. Make the script executable:

    ```bash
    sudo chmod 777 jenkins.sh
    ```

4. Run the script to install Jenkins:

    ```bash
    sudo su
    ./jenkins.sh
    ```

5. Access Jenkins at `http://<EC2_Public_IP>:8080`.
6. Retrieve the initial admin password:

    ```bash
    sudo cat /var/lib/jenkins/secrets/initialAdminPassword
    ```

## Install Docker

1. Update package information:

    ```bash
    sudo apt-get update
    ```

2. Install Docker:

    ```bash
    sudo apt-get install docker.io -y
    ```

3. Add your user to the `docker` group:

    ```bash
    sudo usermod -aG docker $USER
    newgrp docker
    ```

4. Adjust permissions for Docker socket:

    ```bash
    sudo chmod 777 /var/run/docker.sock
    ```

## Set Up SonarQube (Optional)

1. Create a SonarQube container:

    ```bash
    docker run -d --name sonar -p 9000:9000 sonarqube:lts-community
    ```

## Install Trivy, Kubectl, and Terraform

1. Create a `script.sh` file with the following content:

    ```bash
    sudo apt-get install wget apt-transport-https gnupg lsb-release -y
    wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | gpg --dearmor | sudo tee /usr/share/keyrings/trivy.gpg > /dev/null
    echo "deb [signed-by=/usr/share/keyrings/trivy.gpg] https://aquasecurity.github.io/trivy-repo/deb $(lsb_release -sc) main" | sudo tee -a /etc/apt/sources.list.d/trivy.list
    sudo apt-get update
    sudo apt-get install trivy -y

    # Install Terraform
    sudo apt install wget -y
    wget -O- https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
    echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
    sudo apt update && sudo apt install terraform

    # Install kubectl
    sudo apt update
    sudo apt install curl -y
    curl -LO https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl
    sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
    kubectl version --client

    # Install AWS CLI
    curl "https://awscli.amazonaws.com


## Install Jenkins

1. Connect to your console (e.g., SSH into your server).
2. Create a `jenkins.sh` script with the following content:

    ```bash
    #!/bin/bash
    sudo apt update -y
    wget -O - https://packages.adoptium.net/artifactory/api/gpg/key/public | tee /etc/apt/keyrings/adoptium.asc
    echo "deb [signed-by=/etc/apt/keyrings/adoptium.asc] https://packages.adoptium.net/artifactory/deb $(awk -F= '/^VERSION_CODENAME/{print$2}' /etc/os-release) main" | tee /etc/apt/sources.list.d/adoptium.list
    sudo apt update -y
    sudo apt install temurin-17-jdk -y
    /usr/bin/java --version
    curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee /usr/share/keyrings/jenkins-keyring.asc > /dev/null
    echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] https://pkg.jenkins.io/debian-stable binary/ | sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null
    sudo apt-get update -y
    sudo apt-get install jenkins -y
    sudo systemctl start jenkins
    ```

3. Make the script executable:

    ```bash
    sudo chmod 777 jenkins.sh
    ```

4. Run the script to install Jenkins:

    ```bash
    sudo su
    ./jenkins.sh
    ```

5. Access Jenkins at `http://<EC2_Public_IP>:8080`.
6. Retrieve the initial admin password:

    ```bash
    sudo cat /var/lib/jenkins/secrets/initialAdminPassword
    ```

---

## Install Docker

1. Update package information:

    ```bash
    sudo apt-get update
    ```

2. Install Docker:

    ```bash
    sudo apt-get install docker.io -y
    ```

3. Add your user to the `docker` group:

    ```bash
    sudo usermod -aG docker $USER
    newgrp docker
    ```

4. Adjust permissions for Docker socket:

    ```bash
    sudo chmod 777 /var/run/docker.sock
    ```

---

## Set Up SonarQube (Optional)

1. Create a SonarQube container:

    ```bash
    docker run -d --name sonar -p 9000:9000 sonarqube:lts-community
    ```

---

## Install Trivy, Kubectl, and Terraform

1. Create a `script.sh` file with the following content:

    ```bash
    sudo apt-get install wget apt-transport-https gnupg lsb-release -y
    wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | gpg --dearmor | sudo tee /usr/share/keyrings/trivy.gpg > /dev/null
    echo "deb [signed-by=/usr/share/keyrings/trivy.gpg] https://aquasecurity.github.io/trivy-repo/deb $(lsb_release -sc) main" | sudo tee -a /etc/apt/sources.list.d/trivy.list
    sudo apt-get update
    sudo apt-get install trivy -y

    # Install Terraform
    sudo apt install wget -y
    wget -O- https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
    echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
    sudo apt update && sudo apt install terraform

    # Install kubectl
    sudo apt update
    sudo apt install curl -y
    curl -LO https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl
    sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
    kubectl version --client

    # Install AWS CLI 
    curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
    sudo apt-get install unzip -y
    unzip awscliv2.zip
    sudo ./aws/install
    ```

## Install Jenkins Plugins
### Open Jenkins and navigate to Manage Jenkins → Manage Plugins → Available Plugins.
Install the following plugins:
* Blue Ocean
* Eclipse Temurin Installer
* SonarQube Scanner
* NodeJs Plugin
* Docker
* Docker Commons
* Docker Pipeline
* Docker API
* Docker Build Step
* OWASP Dependency Check
* Kubernetes
* Kubernetes CLI
* Kubernetes Client API
* Kubernetes Pipeline DevOps Steps

      ```bash
      pipeline{
          agent any
          stages {
              stage('Checkout from Git'){
                  steps{
                      git branch: 'legacy', url: 'https://github.com/Aj7Ay/chatbot-ui.git'
                  }
              }
              stage('Terraform version'){
                   steps{
                       sh 'terraform --version'
                   }
              }
              stage('Terraform init'){
                   steps{
                       dir('Eks-terraform') {
                            sh 'terraform init'
                         }
                   }
              }
              stage('Terraform validate'){
                   steps{
                       dir('Eks-terraform') {
                            sh 'terraform validate'
                         }
                   }
              }
              stage('Terraform plan'){
                   steps{
                       dir('Eks-terraform') {
                            sh 'terraform plan'
                         }
                   }
              }
              stage('Terraform apply/destroy'){
                   steps{
                       dir('Eks-terraform') {
                            sh 'terraform ${action} --auto-approve'
                         }
                   }
              }
          }
      }
       ```

### Create Job for chatbot clone

Add this stage to Pipeline Script

      ```bash
      pipeline{
          agent any
          tools{
              jdk 'jdk17'
              nodejs 'node19'
          }
          environment {
              SCANNER_HOME=tool 'sonar-scanner'
          }
          stages {
              stage('Checkout from Git'){
                  steps{
                      git branch: 'legacy', url: 'https://github.com/Aj7Ay/chatbot-ui.git'
                  }
              }
              stage('Install Dependencies') {
                  steps {
                      sh "npm install"
                  }
              }
              stage("Sonarqube Analysis "){
                  steps{
                      withSonarQubeEnv('sonar-server') {
                          sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=Chatbot \
                          -Dsonar.projectKey=Chatbot '''
                      }
                  }
              }
              stage("quality gate"){
                 steps {
                      script {
                          waitForQualityGate abortPipeline: false, credentialsId: 'Sonar-token'
                      }
                  }
              }
              stage('OWASP FS SCAN') {
                  steps {
                      dependencyCheck additionalArguments: '--scan ./ --disableYarnAudit --disableNodeAudit', odcInstallation: 'DP-Check'
                      dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
                  }
              }
              stage('TRIVY FS SCAN') {
                  steps {
                      sh "trivy fs . > trivyfs.json"
                  }
              }
              stage("Docker Build & Push"){
                  steps{
                      script{
                         withDockerRegistry(credentialsId: 'docker', toolName: 'docker'){
                             sh "docker build -t chatbot ."
                             sh "docker tag chatbot sevenajay/chatbot:latest "
                             sh "docker push sevenajay/chatbot:latest "
                          }
                      }
                  }
              }
              stage("TRIVY"){
                  steps{
                      sh "trivy image sevenajay/chatbot:latest > trivy.json"
                  }
              }
              stage ("Remove container") {
                  steps{
                      sh "docker stop chatbot | true"
                      sh "docker rm chatbot | true"
                   }
              }
              stage('Deploy to container'){
                  steps{
                      sh 'docker run -d --name chatbot -p 3000:3000 sevenajay/chatbot:latest'
                  }
              }
          }
      ```

   ### Dont forgot to update the Image in chatbot-ui yml file

   ```bash
   pipeline{
       agent any
       tools{
           jdk 'jdk17'
           nodejs 'node19'
       }
       environment {
           SCANNER_HOME=tool 'sonar-scanner'
       }
       stages {
           stage('Checkout from Git'){
               steps{
                   git branch: 'legacy', url: 'https://github.com/Aj7Ay/chatbot-ui.git'
               }
           }
           stage('Install Dependencies') {
               steps {
                   sh "npm install"
               }
           }
           stage("Sonarqube Analysis "){
               steps{
                   withSonarQubeEnv('sonar-server') {
                       sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=Chatbot \
                       -Dsonar.projectKey=Chatbot '''
                   }
               }
           }
           stage("quality gate"){
              steps {
                   script {
                       waitForQualityGate abortPipeline: false, credentialsId: 'Sonar-token'
                   }
               }
           }
           stage('OWASP FS SCAN') {
               steps {
                   dependencyCheck additionalArguments: '--scan ./ --disableYarnAudit --disableNodeAudit', odcInstallation: 'DP-Check'
                   dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
               }
           }
           stage('TRIVY FS SCAN') {
               steps {
                   sh "trivy fs . > trivyfs.json"
               }
           }
           stage("Docker Build & Push"){
               steps{
                   script{
                      withDockerRegistry(credentialsId: 'docker', toolName: 'docker'){
                          sh "docker build -t chatbot ."
                          sh "docker tag chatbot sevenajay/chatbot:latest "
                          sh "docker push sevenajay/chatbot:latest "
                       }
                   }
               }
           }
           stage("TRIVY"){
               steps{
                   sh "trivy image sevenajay/chatbot:latest > trivy.json"
               }
           }
           stage ("Remove container") {
               steps{
                   sh "docker stop chatbot | true"
                   sh "docker rm chatbot | true"
                }
           }
           stage('Deploy to container'){
               steps{
                   sh 'docker run -d --name chatbot -p 3000:3000 sevenajay/chatbot:latest'
               }
           }
           stage('Deploy to kubernets'){
               steps{
                   script{
                       withKubeConfig(caCertificate: '', clusterName: '', contextName: '', credentialsId: 'k8s', namespace: '', restrictKubeConfigAccess: false, serverUrl: '') {
                          sh 'kubectl apply -f k8s/chatbot-ui.yaml'
                     }
                   }
               }
           }
       }
   ```
