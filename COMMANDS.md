# AWS ECS Fargate Lab — Commands and Explanations

**Lab:** Secure Container Deployment on ECS Fargate  
**Region:** us-east-1 (N. Virginia)

## 1. Clone the GitHub Repository

```bash
git clone https://github.com/pluralsight-cloud/Lab-Secure-Container-Deployment-on-ECS-Fargate.git
```

**Explanation:** Downloads the project files from GitHub to AWS CloudShell.

## 2. Navigate to the Project Directory

```bash
cd Lab-Secure-Container-Deployment-on-ECS-Fargate
```

**Explanation:** Opens the project directory so we can work with the application files.

## 3. View the Python Application

```bash
cat app.py
```

**Explanation:** Displays the Python source code to understand how the application works and reads database credentials.

## 4. View the Dockerfile

```bash
cat Dockerfile
```

**Explanation:** Displays the Dockerfile, which contains the instructions for building the Docker container image.

## 5. Retrieve the AWS Account ID

```bash
ACCOUNT_ID=$(aws sts get-caller-identity --query Account --output text)
```

**Explanation:** Retrieves the current AWS account ID and stores it in a variable for use in later commands.

## 6. Set the AWS Region

```bash
REGION=us-east-1
```

**Explanation:** Sets the AWS region to N. Virginia for the deployment.

## 7. Authenticate Docker with Amazon ECR

```bash
aws ecr get-login-password --region $REGION | docker login --username AWS --password-stdin $ACCOUNT_ID.dkr.ecr.$REGION.amazonaws.com
```

**Explanation:** Authenticates Docker with Amazon Elastic Container Registry (ECR), allowing us to push our Docker image to the private repository.

## 8. Build the Docker Image

```bash
docker build -t secure-fargate-app .
```

**Explanation:** Builds a Docker image using the Dockerfile in the current directory and names it `secure-fargate-app`.

## 9. Tag the Docker Image

```bash
docker tag secure-fargate-app:latest $ACCOUNT_ID.dkr.ecr.$REGION.amazonaws.com/secure-fargate-app:latest
```

**Explanation:** Tags the Docker image with the ECR repository address so it can be uploaded to AWS.

## 10. Push the Docker Image to Amazon ECR

```bash
docker push $ACCOUNT_ID.dkr.ecr.$REGION.amazonaws.com/secure-fargate-app:latest
```

**Explanation:** Uploads the Docker image to Amazon ECR, where it can be accessed by Amazon ECS Fargate.

**Note:** The initial push failed because the ECR repository did not exist. I created the private repository `secure-fargate-app` through the AWS Console and repeated the push successfully.

---

## Additional AWS Configuration

The following steps were completed through the AWS Management Console rather than terminal commands.

**IAM Roles**

- Created `ecsTaskExecutionRole` to allow ECS to pull the Docker image and send logs to CloudWatch.
- Attached `AmazonECSTaskExecutionRolePolicy`.
- Added the `SecretsManagerReadAccess` inline policy.
- Created `ecsTaskRole` for the running application.

**AWS Secrets Manager**

- Configured the `prod/db-credentials` secret.
- Used the secret to provide database credentials to the application.

**Amazon ECS Fargate**

- Created the `secure-fargate-app` task definition.
- Configured the Docker image, CPU, memory, IAM roles, and secret injection.
- Created the `secure-fargate-svc` service.
- Configured networking and deployed one Fargate task.

**Amazon CloudWatch**

- Verified that the application logs were generated successfully.
- Confirmed that the secret was injected into the container.
- Verified the application's heartbeat messages.

## Final Result

The Docker image was successfully uploaded to Amazon ECR and deployed using Amazon ECS Fargate.

The container successfully retrieved its credentials through AWS Secrets Manager, and the application logs were available in Amazon CloudWatch.
