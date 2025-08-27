# Telegram MTProto Proxy with Docker

This guide provides a comprehensive walkthrough for setting up a secure and easy-to-manage Telegram MTProto proxy using the official Docker image. This is the most reliable method as it avoids compilation issues and dependency conflicts.

## Prerequisites

1.  A server or VPS with a public IP address.
2.  **Docker** installed on the server.
3.  Root or `sudo` access.
4.  **Crucially**, access to your cloud provider's firewall settings (e.g., AWS Security Groups) to open a port.

---
## Part 1: Server Setup

### 1. Generate a Proxy Secret
The proxy requires a secret key for authentication. Generate a random 32-character hexadecimal string with this command and copy the output.

```bash
openssl rand -hex 16
2. Run the MTProto Proxy Container
Deploy the official proxy using a single docker run command.

Replace YOUR_SECRET_HERE with the secret you just generated.

You can change the port 443 to another port if you wish (e.g., 8888). Port 443 is often a good choice as it's typically allowed on most networks.

Bash

docker run -d \
  --name=mtprotoproxy \
  -p 443:443 \
  --restart=always \
  -e SECRET=YOUR_SECRET_HERE \
  telegrammessenger/proxy:latest
3. Configure Your Firewall
You must open the port you chose (e.g., 443) in two places.

On the Server's Firewall (if active):

Bash

# Use the port you chose in the docker run command
sudo ufw allow 443/tcp
On Your Cloud Provider's Firewall:
Log in to your cloud provider's dashboard (AWS, Google Cloud, Oracle, etc.). Find the "Security Groups" or "Network Rules" for your server and add a new inbound rule to allow TCP traffic on port 443.

Part 2: Get Your Connection Link
The container automatically generates the tg:// link you need to connect.

Check the container logs:

Bash

docker logs mtprotoproxy
Find and copy the link: Look for the output that looks like this and copy the entire tg:// link.

[*]   tg:// link for secret 1 auto configuration: tg://proxy?server=YOUR_SERVER_IP&port=443&secret=YOUR_SECRET
Connect in Telegram: Send the tg:// link to yourself (e.g., in "Saved Messages") and tap on it. Telegram will automatically prompt you to add and enable the proxy.

Part 3: Managing the Container
Here are some basic commands to manage your proxy.

View Logs:

Bash

docker logs mtprotoproxy
Stop the Proxy:

Bash

docker stop mtprotoproxy
Start the Proxy:

Bash

docker start mtprotoproxy
Remove the Proxy:
First stop it, then remove it.

Bash

docker stop mtprotoproxy
docker rm mtprotoproxy
