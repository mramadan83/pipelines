# Dockerfile for .NET Core application
FROM registry.access.redhat.com/ubi8/dotnet-aspnet:8.0

# Set working directory
WORKDIR /app

# Copy .NET application binaries
COPY ./publish /app

# Expose port
EXPOSE 5000

# Run .NET Core application
ENTRYPOINT ["dotnet", "MyApp.dll"]
