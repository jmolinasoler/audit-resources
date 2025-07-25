# Stage 1: Build Aderyn
FROM rustlang/rust:nightly-slim AS builder

WORKDIR /app

# Install system dependencies needed for Aderyn
RUN apt-get update && apt-get install -y --no-install-recommends \
    build-essential \
    pkg-config \
    libssl-dev \
    git \
    ca-certificates \
    curl \
    && rm -rf /var/lib/apt/lists/*

# Download and run the aderyn installer
RUN curl --proto '=https' --tlsv1.2 -LsSf https://github.com/cyfrin/aderyn/releases/latest/download/aderyn-installer.sh -o aderyn-installer.sh && \
    chmod +x aderyn-installer.sh && \
    ./aderyn-installer.sh

# Find where the installer actually placed the binary and verify it exists
RUN find /root -name "aderyn" -type f -executable 2>/dev/null || \
    find /usr -name "aderyn" -type f -executable 2>/dev/null || \
    find / -name "aderyn" -type f -executable 2>/dev/null | head -5

# Check common installation paths and list them
RUN ls -la /root/.cargo/bin/ 2>/dev/null || echo "No .cargo/bin directory" && \
    ls -la /usr/local/bin/ 2>/dev/null || echo "No /usr/local/bin directory" && \
    which aderyn || echo "aderyn not in PATH"

# Copy the binary to a known location
RUN if [ -f /root/.cargo/bin/aderyn ]; then \
        cp /root/.cargo/bin/aderyn /usr/local/bin/aderyn; \
    elif [ -f /usr/local/bin/aderyn ]; then \
        echo "Binary already in /usr/local/bin/"; \
    else \
        ADERYN_PATH=$(find / -name "aderyn" -type f -executable 2>/dev/null | head -1); \
        if [ -n "$ADERYN_PATH" ]; then \
            cp "$ADERYN_PATH" /usr/local/bin/aderyn; \
        else \
            echo "ERROR: aderyn binary not found!" && exit 1; \
        fi; \
    fi

# Stage 2: Create the final, lightweight image
FROM debian:bookworm-slim

# Install runtime dependencies
RUN apt-get update && apt-get install -y --no-install-recommends \
    ca-certificates \
    && rm -rf /var/lib/apt/lists/*

# Set working directory
WORKDIR /aderyn

# Copy the aderyn binary from the builder stage
COPY --from=builder /usr/local/bin/aderyn /usr/local/bin/aderyn

# Make sure the binary is executable
RUN chmod +x /usr/local/bin/aderyn

# Add a non-root user for security
RUN useradd -ms /bin/bash aderynuser && \
    chown -R aderynuser:aderynuser /aderyn

USER aderynuser

# Set the default command to run Aderyn
ENTRYPOINT ["/usr/local/bin/aderyn"]