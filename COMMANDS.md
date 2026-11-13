# AWS ECS Fargate — Commands

### 1. Clone Repository
```bash
git clone https://github.com/pluralsight-cloud/Lab-Secure-Container-Deployment-on-ECS-Fargate.git
```
Downloads the project from GitHub.

### 2. Open Project Directory
```bash
cd Lab-Secure-Container-Deployment-on-ECS-Fargate
```
Navigates to the project folder.

### 3. View Python Application
```bash
cat app.py
```
Displays the Python application code.

### 4. View Dockerfile
```bash
cat Dockerfile
```
Displays the Docker image instructions.

### 5. Get AWS Account ID
```bash
ACCOUNT_ID=$(aws sts get-caller-identity --query Account --output text)
```
Stores the AWS account ID in a variable.

### 6. Set AWS Region
```bash
REGION=us-east-1
```
Sets the deployment region.

### 7. Authenticate Docker with ECR
```bash
aws ecr get-login-password --region $REGION | docker login --username AWS --password-stdin $ACCOUNT_ID.dkr.ecr.$REGION.amazonaws.com
```
Connects Docker to Amazon ECR.

### 8. Build Docker Image
```bash
docker build -t secure-fargate-app .
```
Builds the Docker image.

### 9. Tag Docker Image
```bash
docker tag secure-fargate-app:latest $ACCOUNT_ID.dkr.ecr.$REGION.amazonaws.com/secure-fargate-app:latest
```
Tags the image for Amazon ECR.

### 10. Push Docker Image
```bash
docker push $ACCOUNT_ID.dkr.ecr.$REGION.amazonaws.com/secure-fargate-app:latest
```
Uploads the image to Amazon ECR.
