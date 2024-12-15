# Odoo Multi-Version Docker Setup

This repository contains a Docker Compose configuration for running multiple versions of Odoo (13.0 through 18.0) simultaneously. Each Odoo instance has its own PostgreSQL database.

## Prerequisites

- Linux/Unix-based system
- Docker Engine
- Docker Compose

## Installation Steps

### 1. Install Docker Engine

```bash
# Remove old versions if any exist
sudo apt-get remove docker docker-engine docker.io containerd runc

# Update package index
sudo apt-get update

# Install prerequisites
sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release

# Add Docker's official GPG key
sudo mkdir -m 0755 -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

# Set up the repository
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# Install Docker Engine
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

### 2. Configure Docker

```bash
# Start and enable Docker service
sudo systemctl start docker
sudo systemctl enable docker

# Add your user to docker group
sudo usermod -aG docker $USER

# Log out and log back in, or run:
newgrp docker
```

### 3. Setup Project

1. Clone this repository
2. Create required directories:
   ```bash
   mkdir -p v{13,14,15,16,17,18}/{addons,custom-addons,enterprise}
   ```
3. Configure environment variables in `.env` file (optional)
4. Start all services:
   ```bash
   docker compose up -d
   ```

## Configuration
### Environment Variables (.env)
```
# Ports for different Odoo versions
ODOO18_PORT=8018
ODOO17_PORT=8017
ODOO16_PORT=8016
ODOO15_PORT=8015
ODOO14_PORT=8014
ODOO13_PORT=8013

# Database credentials
DB_USER=odoo
DB_PASSWORD=odoo
```

## Directory Structure
```
.
├── docker-compose.yml
├── .env
├── .gitignore
├── v13
│   ├── addons/
│   ├── custom-addons/
│   └── enterprise/
├── v14/
├── v15/
├── v16/
├── v17/
└── v18/
```

## Usage

### Available Services
After starting, you can access the different Odoo instances at:
- Odoo 18.0: http://localhost:8018
- Odoo 17.0: http://localhost:8017
- Odoo 16.0: http://localhost:8016
- Odoo 15.0: http://localhost:8015
- Odoo 14.0: http://localhost:8014
- Odoo 13.0: http://localhost:8013

### Common Commands
```bash
# Start all versions
docker compose up -d

# Start specific version
docker compose up -d odoo16 db16

# View logs
docker compose logs -f odoo16

# Stop all services
docker compose down
```

### Custom Modules
Each version has its own custom modules directory:
- Place modules in `./vXX/custom-addons/`
- Restart the container: `docker compose restart odoo16`
- Update module list in Odoo UI

### Enterprise Modules
Each version has its own enterprise directory:
- Place enterprise modules in `./vXX/enterprise/`
- Requires valid Odoo Enterprise subscription
- Restart container after adding modules

## Troubleshooting

If you encounter credential errors, try:
```bash
# Clear Docker credentials
rm -rf ~/.docker/config.json
docker logout

# Configure Docker to use simple credential store
echo '{"credsStore":""}' > ~/.docker/config.json
```

## Notes
- Default master password: admin
- Default login: admin/admin
- Each version has its own PostgreSQL database
- Enterprise features require valid subscription
- Data persistence is handled through Docker volumes
```

This updated README now includes:
- Detailed Docker installation steps
- Proper configuration instructions
- Troubleshooting section
- All the useful information from the original README
- Updated commands to use `docker compose` instead of `docker-compose`
sabry youssef
00201000059085
