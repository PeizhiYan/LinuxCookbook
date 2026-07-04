# Tailscale VPN

A quick reference for everyday Tailscale commands — connecting, checking status, sharing a local service privately (Serve), and exposing one to the public internet (Funnel).

> **Tailscale** is a mesh VPN that puts all your devices on one private network (a *tailnet*) using WireGuard. Each device gets a stable `100.x.y.z` IP and a MagicDNS name like `mymachine.tailnet-name.ts.net`.

---

## Basics

```bash
# Connect / bring the device online
tailscale up

# Show your connection + all peers in the tailnet
tailscale status

# Just your own Tailscale IPs (IPv4 + IPv6)
tailscale ip

# Check the daemon is healthy
tailscale netcheck

# Disconnect (stays logged in)
tailscale down

# Bring it back up
tailscale up

# Log out completely (removes the device from your tailnet)
tailscale logout
```

> **macOS note:** if you installed from the App Store or as the Standalone app, the `tailscale` CLI isn't on your PATH by default. Add an alias:
> ```bash
> alias tailscale="/Applications/Tailscale.app/Contents/MacOS/Tailscale"
> ```

---

## Connecting to other devices

Once two devices are on the same tailnet, just use their Tailscale IP or MagicDNS name like you would any normal host:

```bash
ssh user@mymachine.tailnet-name.ts.net   # SSH over the tailnet
ping 100.101.102.103                      # ping by Tailscale IP
```

Find a device's name/IP with `tailscale status`.

---

## Tailscale Serve — share a local service *privately*

Serve exposes a local port to **other devices in your tailnet only** (not the public internet), over HTTPS.

```bash
# Proxy http://localhost:3000 to your tailnet (foreground; Ctrl+C to stop)
tailscale serve 3000

# Run it in the background so it persists after you close the terminal
tailscale serve --bg 3000

# See what's currently being served
tailscale serve status

# Stop serving a port
tailscale serve off
```

Your service becomes reachable at `https://yourmachine.tailnet-name.ts.net` — but **only to devices logged into your tailnet**.

---

## Tailscale Funnel — expose a local service to the *public internet*

Funnel is like Serve, but the URL is reachable by **anyone on the internet** (no Tailscale account needed on their end). Good for sharing a dev server, demo, or webhook receiver.

```bash
tailscale funnel --bg 888
```

That's it. This proxies your local `http://localhost:888` to a public HTTPS URL at your machine's name, something like `https://macbookpro.tail25f331.ts.net`.

The first time you run it, if Funnel/HTTPS isn't enabled on your tailnet yet, the CLI prints a login URL — click it, approve, and it auto-configures everything (certs + the Funnel permission). No manual policy editing.

Check it's live and get your URL:

```bash
tailscale funnel status
```

To take it back down later:

```bash
tailscale funnel off
```

### Funnel notes & gotchas

- **Public port is always 443 / 8443 / 10000** — not your local port. People visit `https://yourmachine.tailnet-name.ts.net` with no port in the URL. The local port (e.g. 888) is just where Funnel forwards *to*.
- **HTTPS only.** Funnel works over TLS; certs are auto-provisioned. The simple `tailscale funnel <port>` form expects your local service to be a normal HTTP server.
- **It's genuinely public.** Anyone with the URL can reach it. Make sure your app has its own authentication, and run `tailscale funnel off` when you're done.
- **Serve vs Funnel on the same port:** whichever command you ran most recently wins. A port is private if `serve` was last, public if `funnel` was last.

---

## Exit nodes (route all traffic through another device)

```bash
# List devices advertised as exit nodes
tailscale exit-node list

# Route your internet traffic through one
tailscale set --exit-node=<exit-node-ip-or-name>

# Stop using an exit node
tailscale set --exit-node=
```

---

## Troubleshooting

### "changing settings via 'tailscale up' requires mentioning all non-default flags"

`tailscale up` won't silently drop settings you previously set. Either re-state them all, or reset to defaults and re-add only what you want:

```bash
# Reset everything to default, then re-apply just the flags you care about
tailscale up --reset --accept-routes
```

> `--reset` clears *all* non-default settings, then the flags you list override that. Useful for escaping a stuck config (e.g. an orphaned `--exit-node-allow-lan-access` left over without an exit node).

### Other handy checks

```bash
tailscale status          # who's online, am I connected?
tailscale netcheck        # NAT / connectivity diagnostics
tailscale ping <device>   # test direct connection to a peer
tailscale version         # client version
```

---

## Quick command reference

| Command | What it does |
| --- | --- |
| `tailscale up` | Connect to the tailnet |
| `tailscale down` | Disconnect (stay logged in) |
| `tailscale status` | Show connection + peers |
| `tailscale ip` | Show your Tailscale IPs |
| `tailscale serve --bg <port>` | Share a local port *privately* (tailnet) |
| `tailscale funnel --bg <port>` | Share a local port *publicly* (internet) |
| `tailscale serve off` / `funnel off` | Stop sharing |
| `tailscale up --reset` | Reset settings to default |
| `tailscale logout` | Remove device from tailnet |
