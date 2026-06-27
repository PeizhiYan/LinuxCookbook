[return to table](../README.md)

---

# Cloudflare Tunnel

A quick reference for managing Cloudflare Tunnels with `cloudflared`.

> **Cloudflare Tunnel** exposes local services to the public internet through Cloudflare's network — no open firewall ports needed. Traffic goes through an outbound connection from your machine to Cloudflare.

---

## Setup (first time only)

```bash
# Install cloudflared (Debian/Raspberry Pi)
curl -L https://pkg.cloudflare.com/cloudflare-main.gpg | sudo tee /usr/share/keyrings/cloudflare-archive-keyring.gpg >/dev/null
echo "deb [signed-by=/usr/share/keyrings/cloudflare-archive-keyring.gpg] https://pkg.cloudflare.com/cloudflared any main" | sudo tee /etc/apt/sources.list.d/cloudflared.list
sudo apt update && sudo apt install cloudflared

# Log in to your Cloudflare account (opens a browser URL)
cloudflared tunnel login
```

---

## Tunnel management

```bash
# Create a tunnel
cloudflared tunnel create <name>

# List all tunnels
cloudflared tunnel list

# Show tunnel details (connections, config)
cloudflared tunnel info <name>

# Delete a tunnel
cloudflared tunnel delete <name>
```

---

## Config file

Create `~/.cloudflared/config.yml`:

```yaml
tunnel: <tunnel-id>
credentials-file: /home/pi/.cloudflared/<tunnel-id>.json

ingress:
  - hostname: home.yourdomain.com
    service: http://localhost:8080
  - service: http_status:404
```

---

## DNS routing

```bash
# Point a subdomain to your tunnel (creates a CNAME in Cloudflare DNS)
cloudflared tunnel route dns <tunnel-name> home.yourdomain.com
```

---

## Running the tunnel

```bash
# Run in foreground (Ctrl+C to stop)
cloudflared tunnel run <name>

# Install as a background system service
sudo cloudflared service install
sudo systemctl start cloudflared
sudo systemctl enable cloudflared

# Check service status
sudo systemctl status cloudflared

# Stop / restart the service
sudo systemctl stop cloudflared
sudo systemctl restart cloudflared
```

---

## Quick command reference

| Command | What it does |
| --- | --- |
| `cloudflared tunnel login` | Authenticate with Cloudflare |
| `cloudflared tunnel create <name>` | Create a new tunnel |
| `cloudflared tunnel list` | List all tunnels |
| `cloudflared tunnel info <name>` | Show tunnel details |
| `cloudflared tunnel delete <name>` | Delete a tunnel |
| `cloudflared tunnel route dns <name> <hostname>` | Map a domain to the tunnel |
| `cloudflared tunnel run <name>` | Run tunnel in foreground |
| `sudo cloudflared service install` | Install as system service |
| `sudo systemctl restart cloudflared` | Restart the service |
