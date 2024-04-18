# Use the .NET runtime as the base image for running the application
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app
EXPOSE 80

# Use the .NET SDK for building the project
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src
COPY ["WeatherAPI/WeatherAPI.csproj", "WeatherAPI/"]
RUN dotnet restore "WeatherAPI/WeatherAPI.csproj"
COPY . .
WORKDIR "/src/WeatherAPI"
RUN dotnet build "WeatherAPI.csproj" -c Release -o /app/build

# Publish the project
FROM build AS publish
RUN dotnet publish "WeatherAPI.csproj" -c Release -o /app/publish /p:UseAppHost=false

# Final image stage
FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "WeatherAPI.dll"]
