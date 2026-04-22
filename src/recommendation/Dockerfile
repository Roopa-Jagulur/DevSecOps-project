FROM python:3.12-slim-bookworm AS base

# Create a non-root user and group
RUN groupadd -r dsouser && useradd -r -g dsouser dsouser

WORKDIR /usr/src/app

# Copy requirements first (better layer caching)
COPY requirements.txt ./

# Install dependencies
RUN pip install --upgrade pip && \
    pip install --no-cache-dir -r requirements.txt && \
    pip install --no-cache-dir opentelemetry-distro

# Copy application code
COPY . .

# Install OpenTelemetry dependencies
#RUN opentelemetry-bootstrap -a install

# Change ownership of app directory
RUN chown -R dsouser:dsouser /usr/src/app

# Switch to non-root user
USER dsouser

# Environment variable
ENV RECOMMENDATION_PORT=1010

# Run the app
ENTRYPOINT ["python", "recommendation_server.py"]