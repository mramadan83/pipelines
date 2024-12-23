# Dockerfile for Spring Boot application
FROM registry.access.redhat.com/ubi8/openjdk-17:1.18

# Set working directory
WORKDIR /app

# Copy Spring Boot JAR file
COPY target/*.jar app.jar

# Expose port
EXPOSE 8080

# Run Spring Boot application
ENTRYPOINT ["java", "-jar", "app.jar"]
