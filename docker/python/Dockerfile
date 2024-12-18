# Dockerfile for Python application
FROM registry.access.redhat.com/ubi8/python-311:1-76

# Set working directory
WORKDIR /app

# Copy requirements and install dependencies
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copy Python source code
COPY . .

# Expose port
EXPOSE 5000

# Run the application
CMD ["python", "app.py"]
