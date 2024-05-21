# DevSecOps for OpenAI Chatbot UI Deployment

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

