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
