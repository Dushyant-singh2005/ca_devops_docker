# Stage 3 hardened Dockerfile — python:3.12-slim, non-root user, pinned version
# Replaces python:3.9 (Stage 1) which contained many CRITICAL/HIGH CVEs.
FROM python:3.12-slim

# Build argument for application version (set at build time)
ARG APP_VERSION=dev
ENV APP_VERSION=${APP_VERSION}

# Set working directory
WORKDIR /app

# Create a non-root user and group for security
RUN groupadd --gid 1001 appgroup && \
    useradd --uid 1001 --gid appgroup --no-create-home --shell /bin/false appuser

# Copy requirements first — install step is cached between builds
COPY requirements.txt .

# Install dependencies as root (needed for pip), then drop privileges
RUN pip install --no-cache-dir --upgrade pip && \
    pip install --no-cache-dir -r requirements.txt

# Copy application code
COPY app.py .

# Change ownership of app files to non-root user
RUN chown -R appuser:appgroup /app

# Switch to non-root user
USER appuser

# Expose port 5000
EXPOSE 5000

# Start the application
CMD ["python", "app.py"]
