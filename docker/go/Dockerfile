# Dockerfile for Go application
FROM registry.access.redhat.com/ubi8/go-toolset:1-76

# Set working directory
WORKDIR /app

# Copy Go source code
COPY . .

# Build the application
RUN go build -o main .

# Expose port
EXPOSE 8080

# Run the application
CMD ["./main"]
