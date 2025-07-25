# Aderyn Docker Image

A lightweight Docker image for running [Aderyn](https://github.com/cyfrin/aderyn), a Solidity static analyzer.

## Quick Start

### Build
```bash
docker build -t aderyn-light .
# or with Podman
podman build -t aderyn-light .
```

### Run
```bash
# Analyze current directory
docker run --rm -v $(pwd):/workspace aderyn-light /workspace

# With output file
docker run --rm -v $(pwd):/workspace aderyn-light /workspace --output /workspace/report.md

# Podman (with SELinux label)
podman run --user 0:0 --rm -v $(pwd):/workspace:Z aderyn-light /workspace --output /workspace/report.md
```

## Requirements
- Solidity project with `foundry.toml` or similar configuration
- Docker or Podman installed

## Examples
```bash
# Basic audit
docker run --rm -v ~/my-solidity-project:/workspace aderyn-light /workspace

# Custom output location
docker run --rm -v $(pwd):/workspace aderyn-light /workspace -o /workspace/audit-report.json
```
