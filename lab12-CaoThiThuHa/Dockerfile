FROM python:3.11-slim

WORKDIR /app

# Install system dependencies
RUN apt-get update && apt-get install -y \
    build-essential \
    && rm -rf /var/lib/apt/lists/*

# Copy requirements and install
COPY codebase/requirements.txt .
RUN pip install --default-timeout=1000 --no-cache-dir -r requirements.txt

# Copy the rest of the codebase
COPY codebase /app/codebase

# Set environment variables
ENV PYTHONPATH=/app/codebase
ENV PORT=8000

# Expose port
EXPOSE $PORT

# Command to run the application
CMD uvicorn app.main:app --host 0.0.0.0 --port $PORT --app-dir /app/codebase
