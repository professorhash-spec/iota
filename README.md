IOTA NODE SETUP
```markdown
# IOTA Validator Node Setup Guide

This guide explains how to install, configure, and run a validator node for the IOTA network using HORNET.

---

## Requirements

### System Requirements
- **RAM**: 128 GB
- **CPU**: 24-core
- **Storage**: 4 TB SSD
- **Network**: 1 Gbps
- **OS**: Ubuntu 20.04 or higher

### Dependencies
- Docker and Docker Compose
- Git

---

## Step 1: Install Docker and Docker Compose

### Install Docker
```bash
sudo apt update
sudo apt install -y docker.io
sudo systemctl start docker
sudo systemctl enable docker
```

### Install Docker Compose
```bash
sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
docker-compose --version
```

---

## Step 2: Download HORNET

Clone the HORNET repository:
```bash
git clone https://github.com/iotaledger/hornet.git
cd hornet
```

---

## Step 3: Configure the Node

### Create `config.json`
Create a configuration file named `config.json` with the following content:
```json
{
  "node": {
    "alias": "MyValidatorNode",
    "profile": "validator"
  },
  "network": {
    "identityPrivateKey": "YOUR_PRIVATE_KEY"
  },
  "storage": {
    "path": "data/mainnetdb"
  },
  "protocol": {
    "port": 14600
  }
}
```

Replace `YOUR_PRIVATE_KEY` with the private key generated in Step 6.

---

## Step 4: Run HORNET Using Docker

### Create `docker-compose.yml`
Create a `docker-compose.yml` file with the following content:
```yaml
version: "3.7"
services:
  hornet:
    image: "iotaledger/hornet:latest"
    container_name: hornet_node
    ports:
      - "14600:14600"
      - "8081:8081"
    volumes:
      - ./config.json:/app/config.json
      - ./data:/app/data
    restart: always
```

### Start the Node
Run the following command:
```bash
docker-compose up -d
```

---

## Step 5: Verify Node Status

### View Logs
To check the node logs:
```bash
docker logs -f hornet_node
```

### Access API Dashboard
Open the following URL in your browser:
```
http://<your_IP>:8081
```

---

## Step 6: Set Up Validator Functionality

### Generate Validator Keys
Generate ED25519 keys for your validator:
```bash
hornet tool ed25519-key
```

Copy the generated private key into the `network.identityPrivateKey` section of your `config.json` file.

---

## Step 7: Stake Tokens

To participate as a validator, stake at least 2 million IOTA tokens. Use Firefly Wallet or the IOTA CLI to complete the staking process.

---

## Step 8: Monitor and Update the Node

### Monitor
Use the HORNET API to monitor your node's performance and status.

### Update
To update your node, pull the latest version and restart:
```bash
docker-compose pull
docker-compose up -d
```

---

