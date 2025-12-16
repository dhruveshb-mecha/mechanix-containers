# GUI Container

## Overview
This Docker container is used to build and develop GUI components for the Mechanix project.

## Usage

### Build the Container
```bash
docker build -t mechanix-gui .
```

### Run the Container
```bash
docker run -it mechanix-gui
```

### Development
Mount your local project directory:
```bash
docker run -it -v $(pwd):/workspace mechanix-gui
```

## Features
- Pre-configured build environment for GUI components
- All necessary dependencies included
- Isolated development environment

## Requirements
- Docker installed and running

## Support
For issues or questions, refer to the main Mechanix documentation.