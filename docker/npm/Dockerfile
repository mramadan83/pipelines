# Dockerfile for Angular application
FROM registry.access.redhat.com/ubi8/nodejs-20:1-76

# Set working directory
WORKDIR /app

# Copy package files and install dependencies
COPY package*.json ./
RUN npm install

# Copy Angular application source
COPY . .

# Build Angular application
RUN npm run build --prod

# Expose port
EXPOSE 4200

# Start the application
CMD ["npx", "http-server", "dist/app-name", "-p", "4200"]
