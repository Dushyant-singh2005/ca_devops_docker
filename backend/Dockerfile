# Stage 1 Dockerfile — as required by the assessment (python:3.9 base)
# Stage 3 will replace this with a hardened, slim/alpine image.
FROM python:3.9

# Set working directory
WORKDIR /app

# Copy requirements first — install step is cached between builds
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copy application code
COPY app.py .

# Expose port 5000
EXPOSE 5000

# Start the application
CMD ["python", "app.py"]
