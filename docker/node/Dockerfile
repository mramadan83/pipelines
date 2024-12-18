# Dockerfile for Node.js application
FROM registry.access.redhat.com/ubi8/nodejs-20:1-76

# Set working directory
WORKDIR /app

# Install dependencies
COPY package*.json ./
RUN npm install

# Copy application source
COPY . .

# Expose port
EXPOSE 3000

# Start Node.js application
CMD ["node", "server.js"]
