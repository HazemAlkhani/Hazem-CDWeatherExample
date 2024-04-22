# Base image for the SDK to build the projects
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src

# Copy and restore the Models project
COPY ["Models/Models.csproj", "Models/"]
RUN dotnet restore "Models/Models.csproj"

# Copy and restore the WeatherAPI project
COPY ["WeatherAPI/WeatherAPI.csproj", "WeatherAPI/"]
RUN dotnet restore "WeatherAPI/WeatherAPI.csproj"

# Copy the rest of the source files for both projects
COPY Models/ Models/
COPY WeatherAPI/ WeatherAPI/

# Build the WeatherAPI project
WORKDIR "/src/WeatherAPI"
RUN dotnet build "WeatherAPI.csproj" -c Release -o /app/build

# Define the publish environment
FROM build AS publish
RUN dotnet publish "WeatherAPI.csproj" -c Release -o /app/publish /p:UseAppHost=false

# Define the runtime environment
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "WeatherAPI.dll"]
