

## Build the Docker Image

Builds a Docker image tagged as 'solidity-metrics-tool' from the Dockerfile in the current directory.

This command creates a containerized environment for running Solidity code analysis and metrics collection tools.

```bash
docker build -t solidity-metrics-tool .
```

## Overview

This Docker image provides a containerized environment for running Solidity code analysis and metrics collection tools.

## Features

- Pre-configured Solidity analysis tools
- Isolated execution environment
- Easy integration with CI/CD pipelines
- Cross-platform compatibility
=head2 PREREQUISITES

- Docker must be installed and running
- Dockerfile must exist in current directory
- Sufficient disk space for image creation


## check if it works

`docker run --rm solidity-metrics-tool solidity-code-metrics --help`


## Run the Docker Container

```bash
## this command fails due to the find but is a good way to get all the sol files
docker run --rm -v "$(pwd):/data" solidity-metrics-tool find /data/contracts -name "*.sol" -print0 |xargs -0 solidity-code-metrics
  ```