# Swarm-Node

Here is a step-by-step guide for setting up a node on the Swarm testnet. This guide covers all the necessary commands and configurations needed to get your node up and running without mentioning Nodes Guru.

### Step 1: Prepare Your Server
Make sure you have a fresh Linux server (Ubuntu 20.04 recommended) with at least 2 GB of RAM, 2 CPU cores, and 30 GB of disk space. Update and upgrade the server packages.

```bash
sudo apt update && sudo apt upgrade -y
```

### Step 2: Install Required Dependencies
Install the essential packages and dependencies.

```bash
sudo apt install curl git build-essential -y
```

### Step 3: Install Docker and Docker Compose
Swarm nodes use Docker, so you need to install Docker and Docker Compose.

1. **Install Docker:**
   ```bash
   curl -fsSL https://get.docker.com -o get-docker.sh
   sudo sh get-docker.sh
   ```

2. **Add your user to the Docker group:**
   ```bash
   sudo usermod -aG docker $USER
   ```

3. **Install Docker Compose:**
   ```bash
   sudo apt install docker-compose -y
   ```

### Step 4: Clone the Swarm Repository
Clone the Swarm testnet repository to your server.

```bash
git clone https://github.com/ethersphere/bee.git
cd bee
```

### Step 5: Create Configuration Files
Create a directory for your configuration files.

```bash
mkdir -p ~/.bee
cd ~/.bee
```

Now, create a configuration file:

```bash
nano config.yml
```

Insert the following configuration:

```yaml
api-addr: :1633
p2p-addr: :1634
debug-api-addr: :1635
data-dir: /home/yourusername/.bee
cache-capacity: "1000000"
swap-endpoint: https://rpc.slock.it/goerli
verbosity: 3
```

Make sure to replace `/home/yourusername` with your actual home directory path.

### Step 6: Create a Docker Compose File
Go back to your project directory and create a `docker-compose.yml` file.

```bash
cd ~/bee
nano docker-compose.yml
```

Insert the following content:

```yaml
version: "3.8"
services:
  bee:
    image: ethersphere/bee:latest
    container_name: bee
    restart: always
    ports:
      - "1633:1633"
      - "1634:1634"
      - "1635:1635"
    volumes:
      - ~/.bee:/home/bee/.bee
    command: start --config /home/bee/.bee/config.yml
```

### Step 7: Start Your Swarm Node
Run the following command to start the Docker container:

```bash
docker-compose up -d
```

### Step 8: Check Node Logs
To ensure your node is running correctly, check the logs:

```bash
docker logs -f bee
```

### Step 9: Access the Swarm Debug API
You can interact with your Swarm node via the debug API on port `1635`. To check the status, run:

```bash
curl http://localhost:1635/health
```

### Step 10: Update Your Node
To update your Swarm node, follow these steps:

1. Pull the latest Docker image:
   ```bash
   docker pull ethersphere/bee:latest
   ```

2. Restart the container:
   ```bash
   docker-compose down
   docker-compose up -d
   ```

Your Swarm node is now up and running. Make sure to monitor your logs and keep your node updated.
