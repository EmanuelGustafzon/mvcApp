# Base runtime image
FROM mcr.microsoft.com/dotnet/aspnet:9.0 AS base
ARG APP_UID=1000
USER $APP_UID
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

# Build stage
FROM mcr.microsoft.com/dotnet/sdk:9.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src

# Debug: List files before copying
RUN echo "Checking initial directory structure..." && ls -R /

# Fix: Corrected COPY path
COPY ["mvc-project.csproj", "mvc-project/"]

# Debug: Verify the file exists
RUN ls -R /src

# Fix: Clear NuGet cache before restore
RUN dotnet nuget locals all --clear

# Run restore (with error message if it fails)
RUN dotnet restore "./mvc-project/mvc-project.csproj" || (echo "Restore failed!" && exit 1)

COPY . .
WORKDIR "/src/mvc-project"

# Debug: Check .NET version
RUN dotnet --version

# Fix: Set default build configuration
RUN dotnet build "mvc-project.csproj" -c ${BUILD_CONFIGURATION:-Release} -o /app/build
