FROM python:3.11-slim

WORKDIR /app

# Install system dependencies
RUN apt-get update && apt-get install -y \
    gcc \
    curl \
    && rm -rf /var/lib/apt/lists/*

# Copy requirements and install Python dependencies
COPY ./apps/docs-service/requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copy shared core package
COPY ./packages/core ./core

# Copy application code
COPY ./apps/docs-service .

# Add current directory to Python path to ensure core module is accessible
ENV PYTHONPATH="/app"

# Create non-root user
RUN useradd -m -u 1000 appuser && chown -R appuser:appuser /app
USER appuser

# Expose port
EXPOSE 8084

# Run the application
CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8084"]
