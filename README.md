# Kubernetes-Application-Deployment-with-AWS-EKS-and-ECR
Project

Prerequisite: git, Github Desktop, Visual Studio, Powershell, dotnet7

Step 1) Clone Repo 
```
git clone https://github.com/atulkamble/k8s-App-EKS.git
```
step 2) 

cd k8s-App-EKS

Step 3) Open Solution file within project folder

Step 4) Run Project from Visual Studio

Step 5) If any changes are there, then commit to same repo.

Step 6) Raise Issue.
Reference: https://learn.microsoft.com/en-us/dotnet/core/docker/build-container?tabs=windows&pivots=dotnet-7-0

```
cd k8s-App-EKS
ls
cd k8s-App
ls
dotnet publish -c Release
```
Following Path will be created

k8s-App -> D:\GitHub\k8s-App-EKS\k8s-App\k8s-App\bin\Release\net6.0\k8s-App.dll
k8s-App -> D:\GitHub\k8s-App-EKS\k8s-App\k8s-App\bin\Release\net6.0\publish\

```
Navigate to folder containing project file
```
D:\test\Project\k8s-App-EKS\k8s-App\k8s-App
```
create a docker file from following command
```
New-Item Dockerfile
```
```
code Dockerfile
```
Paste following Code to Dockerfile

```
FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build-env
WORKDIR /App

# Copy everything
COPY . ./
# Restore as distinct layers
RUN dotnet restore
# Build and publish a release
RUN dotnet publish -c Release -o out

# Build runtime image
FROM mcr.microsoft.com/dotnet/aspnet:7.0
WORKDIR /App
COPY --from=build-env /App/out .
ENTRYPOINT ["dotnet", "DotNet.Docker.dll"]
```
📁 docker-working
    └──📂 App
        ├── Dockerfile
        ├── DotNet.Docker.csproj
        ├── Program.cs
        ├──📂 bin
        │   └──📂 Release
        │       └──📂 net7.0
        │           └──📂 publish
        │               ├── DotNet.Docker.deps.json
        │               ├── DotNet.Docker.exe
        │               ├── DotNet.Docker.dll
        │               ├── DotNet.Docker.pdb
        │               └── DotNet.Docker.runtimeconfig.json
        └──📁 obj
            └──...

```
docker build -t counter-image -f Dockerfile .
```
Start Docker Engine and fire following command from terminal/powershell
```
docker images
```

Create Container
```
docker create --name core-counter counter-image
```
list running container
```
docker ps -a
```
Manage Container
```
docker start core-counter
core-counter

docker ps
```
Stop Container
```
docker stop core-counter
core-counter

docker ps
```

Connect to Container
```
docker start core-counter
core-counter

docker attach --sig-proxy=false core-counter
Counter: 7
Counter: 8
Counter: 9
^C

docker attach --sig-proxy=false core-counter
Counter: 17
Counter: 18
Counter: 19
^C
```
delete container
```
docker stop core-counter
```
```
docker run -it --rm counter-image
```
