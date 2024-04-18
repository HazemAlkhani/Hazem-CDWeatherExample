# Base Image for running the application
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

# Building the project
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src

# Copy csproj files first to utilize Docker layer caching for restore step
COPY ["WeatherFrontend/WeatherFrontend.csproj", "WeatherFrontend/WeatherFrontend.csproj"]
COPY ["Models/Models.csproj", "Models/Models.csproj"]

# Restore dependencies
RUN dotnet restore "WeatherFrontend/WeatherFrontend.csproj"
RUN dotnet restore "Models/Models.csproj"

# Copy the remaining source files
COPY . .

# Set the working directory to the WeatherFrontend project
WORKDIR "/src/WeatherFrontend"

# Build the WeatherFrontend project
RUN dotnet build "WeatherFrontend.csproj" -c Release -o /app/build

# Publishing the project
FROM build AS publish
RUN dotnet publish "WeatherFrontend.csproj" -c Release -o /app/publish /p:UseAppHost=false

# Final Image for deployment
FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "WeatherFrontend.dll"]
