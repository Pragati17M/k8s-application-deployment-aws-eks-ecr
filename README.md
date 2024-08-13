# Kubernetes Application Deployment with AWS EKS and ECR

# Deploy a Full Website on Azure Ubuntu VM 

This project demonstrates the deployment of a .NET 7 application on Kubernetes using AWS Elastic Kubernetes Service (EKS) and Docker. Follow the steps below to clone the repository, build and run the application, and deploy it to AWS.

## Prerequisites

- **Git**: Version control system
- **GitHub Desktop**: Git client for desktop
- **Visual Studio**: IDE for .NET development
- **PowerShell**: Command-line shell
- **.NET 7 SDK**: Development platform for building applications

## Steps to Setup and Run the Project

### 1. Clone the Repository
```
git clone https://github.com/atulkamble/k8s-application-deployment-aws-eks-ecr.git
cd k8s-application-deployment-aws-eks-ecr
```

### 2. Open and Run the Project
- Open the solution file within the `k8s-App-EKS` directory using Visual Studio.
- Run the project from Visual Studio to ensure it works correctly.

### 3. Commit Changes
- If you make any changes, commit them to the same repository:
```bash
git add .
git commit -m "Your commit message"
git push origin main
```

### 4. Raise an Issue
- If you encounter any issues, raise them in the repository's issue tracker.
- Reference: [Build a .NET Core Docker Container](https://learn.microsoft.com/en-us/dotnet/core/docker/build-container?tabs=windows&pivots=dotnet-7-0)

## Dockerize the Application

### 1. Build the Application
```bash
cd k8s-App-EKS
dotnet publish -c Release
```
- The following paths will be created:
  - `k8s-App` -> `D:\GitHub\k8s-App-EKS\k8s-App\k8s-App\bin\Release\net7.0\k8s-App.dll`
  - `k8s-App` -> `D:\GitHub\k8s-App-EKS\k8s-App\k8s-App\bin\Release\net7.0\publish\`

### 2. Create Dockerfile
Navigate to the project directory and create a Dockerfile:
```bash
cd D:\GitHub\k8s-App-EKS\k8s-App\k8s-App
New-Item Dockerfile
code Dockerfile
```
Paste the following code into the Dockerfile:
```dockerfile
# Use the official .NET SDK image to build the application
FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build-env
WORKDIR /App

# Copy all files to the container
COPY . ./
# Restore dependencies
RUN dotnet restore
# Build and publish the application
RUN dotnet publish -c Release -o out

# Use the official ASP.NET runtime image for running the application
FROM mcr.microsoft.com/dotnet/aspnet:7.0
WORKDIR /App
COPY --from=build-env /App/out .
ENTRYPOINT ["dotnet", "k8s-App.dll"]
```

### 3. Build and Run Docker Container
- Build the Docker image:
```bash
docker build -t counter-image -f Dockerfile .
```
- Start Docker Engine and list Docker images:
```bash
docker images
```
- Create a container from the image:
```bash
docker create --name core-counter counter-image
```
- List all containers:
```bash
docker ps -a
```
- Manage the container:
```bash
docker start core-counter
docker ps
```
- Stop the container:
```bash
docker stop core-counter
docker ps
```
- Connect to the container:
```bash
docker attach --sig-proxy=false core-counter
```
- Delete the container:
```bash
docker stop core-counter
docker rm core-counter
```
- Run the container interactively:
```bash
docker run -it --rm counter-image
```

## Deploy to AWS ECR

### 1. Configure AWS CLI
- Sign in to AWS IAM, create a user, group, and assign policies.
- Configure AWS CLI:
```bash
aws configure
```
- Authenticate Docker with AWS ECR:
```bash
aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 316290926278.dkr.ecr.us-east-1.amazonaws.com
```

### 2. Tag and Push Docker Image
- Tag the Docker image:
```bash
docker tag counter-image:latest 316290926278.dkr.ecr.us-east-1.amazonaws.com/counter-image:latest
```
- Push the Docker image to ECR:
```bash
docker push 316290926278.dkr.ecr.us-east-1.amazonaws.com/counter-image:latest
```

By following these steps, you will have successfully Dockerized your .NET 7 application, built a Docker image, and pushed it to AWS ECR for deployment on AWS EKS.
