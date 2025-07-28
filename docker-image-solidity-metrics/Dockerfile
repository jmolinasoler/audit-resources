# Use a Debian base for better control over Node.js and Python versions
FROM debian:bullseye-slim

# Set the working directory in the container
WORKDIR /app

# --- Install basic utilities and Node.js (recent LTS) ---
RUN apt-get update -qq && \
    apt-get install -y --no-install-recommends \
        git \
        curl \ 
        python3 \
        python3-pip \
        # Add build essentials if npm needs to compile native modules (good practice)
        build-essential && \
    rm -rf /var/lib/apt/lists/*

# Install Node.js LTS (e.g., Node.js 18 or 20) using NodeSource
# This ensures you get a recent version, not just what's in Debian's repos
# Using Node.js 18 (often a good balance for compatibility)
RUN curl -fsSL https://deb.nodesource.com/setup_18.x | bash - && \
    apt-get install -y nodejs && \
    rm -rf /var/lib/apt/lists/*

# Link python3 to python (common practice for older scripts that expect 'python')
RUN ln -s /usr/bin/python3 /usr/bin/python

# --- Install solidity-metrics (Python tool) ---
# Clone the solidity-metrics repository
RUN git clone https://github.com/ConsenSysDiligence/solidity-metrics.git /app/solidity-metrics

# Change to the solidity-metrics directory for its dependencies
WORKDIR /app/solidity-metrics

# Install the required Python dependencies for solidity-metrics
RUN pip install --no-cache-dir pip setuptools wheel
RUN pip install --no-cache-dir \
    py-solc-ast \
    graphviz \
    matplotlib \
    pandas \
    numpy \
    networkx

# Clean up pip cache
RUN rm -rf ~/.cache/pip

# --- Install solidity-code-metrics (JavaScript tool) ---
# Change back to the main app directory or a new directory for npm packages
WORKDIR /app/
RUN mkdir -p /app/node_modules_metrics # Create a specific directory for npm installs
WORKDIR /app/node_modules_metrics

# Install solidity-code-metrics
RUN npm install solidity-code-metrics

# To ensure the executables are in the PATH when running commands
ENV PATH="/app/node_modules_metrics/node_modules/.bin:${PATH}"

# Final working directory for running commands
WORKDIR /app

# --- Instructions for use ---
# To run ConsenSysDiligence/solidity-metrics:
# docker run --rm -v "$(pwd)/my_solidity_project:/data" <image-name> python /app/solidity-metrics/main.py --src /data

# To run solidity-code-metrics:
# docker run --rm -v "$(pwd)/my_solidity_project:/data" <image-name> solidity-code-metrics --dir /data --help