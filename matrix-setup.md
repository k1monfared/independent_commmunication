# Matrix Self-Hosting Plan (Local PC)

## Overview
Matrix is an open standard for decentralized, real-time communication. The main server implementation is **Synapse** (Python).

## Your Setup Context
- **Hosting**: Local shared desktop PC
- **IP**: Dynamic (needs DDNS)
- **Domain**: Free option preferred
- **Goal**: Family use → Community expansion

## Challenges for Your Setup

### 1. Resource Usage on Shared PC
- Synapse uses 1-4 GB RAM constantly
- Will impact your regular PC usage
- PostgreSQL adds more overhead
- **Verdict**: Problematic for shared PC

### 2. Dynamic IP + Federation
- Matrix federation expects stable endpoints
- Frequent IP changes cause federation issues
- Other servers cache your IP → connection failures
- **Verdict**: Federation will be unreliable

### 3. Free Domain Limitations
- Matrix IDs are permanent: `@user:yourdomain.com`
- If you lose free subdomain, all IDs break
- Free DDNS domains look less trustworthy
- **Verdict**: Works but risky long-term

### 4. Uptime Requirements
- Shared PC = not always on
- Federation messages queue but can be lost
- Users expect messaging to "just work"
- **Verdict**: Poor experience for community use

## If You Still Want Matrix

### Free Domain Options
- **DuckDNS**: `yourname.duckdns.org` (recommended)
- **No-IP**: Free tier with renewals
- **FreeDNS**: `yourname.afraid.org`

### DDNS Setup
1. Create account at duckdns.org
2. Install DDNS updater:
   ```bash
   # Create update script
   echo "*/5 * * * * curl -s 'https://www.duckdns.org/update?domains=YOURDOMAIN&token=YOURTOKEN'" | crontab -
   ```

### Router Configuration
Forward these ports to your PC:
| Port | Purpose |
|------|---------|
| 443 | HTTPS (reverse proxy) |
| 8448 | Matrix federation |

### Minimal Installation (Docker)
```bash
# Install Docker
curl -fsSL https://get.docker.com | sh

# Use docker-compose with Synapse + PostgreSQL + Caddy
git clone https://github.com/AmirDez/matrix-on-premise
cd matrix-on-premise
# Edit .env with your domain
docker-compose up -d
```

### Resource Reduction Tips
- Disable federation (family-only mode)
- Use SQLite (not recommended but lighter)
- Limit media upload sizes
- Run without Element (use mobile apps only)

## Hardware Requirements (Your PC)

| Resource | Impact |
|----------|--------|
| RAM | 2-4 GB constantly used |
| CPU | Moderate, spikes during activity |
| Disk | 10-50+ GB over time |
| Network | Moderate bandwidth |

## Honest Assessment

**For family use on shared PC**: Matrix is overkill and will noticeably slow your computer.

**For community expansion**: Dynamic IP and free domain make federation unreliable. Community members will have poor experience.

**Recommendation**: Consider XMPP instead, or plan to move Matrix to dedicated hardware/VPS later.

## Growth Path: Family → Community

If you start with Matrix anyway:

1. **Family phase**: Disable federation, local network only
2. **Transition**: Get proper domain, move to dedicated hardware
3. **Community phase**: Enable federation, proper setup

## Steps to Deploy (If Proceeding)

1. Set up DDNS (DuckDNS)
2. Configure router port forwarding (443, 8448)
3. Install Docker on your PC
4. Clone docker-compose setup
5. Configure with your DDNS domain
6. Start services
7. Create family accounts
8. Disable open registration

---

## Offline Setup (No Internet During Installation)

Matrix/Synapse has more dependencies than XMPP, making offline setup more complex.

### Packages to Download (Debian/Ubuntu - Native Install)

```bash
# On a machine WITH internet:
mkdir ~/matrix-offline && cd ~/matrix-offline

# Add Matrix repo key and source first, then download
apt-get download matrix-synapse-py3

# Download all dependencies (this list is large)
apt-rdepends matrix-synapse-py3 | grep -v "^ " | xargs apt-get download

# PostgreSQL (recommended)
apt-get download postgresql postgresql-client libpq5

# For reverse proxy
apt-get download nginx
```

**Warning:** Synapse has many Python dependencies. The package list can be 100+ packages.

### Docker Method (Recommended for Offline)

On a machine WITH internet:
```bash
# Pull and save Docker images
docker pull matrixdotorg/synapse:latest
docker pull postgres:15
docker pull vectorim/element-web:latest
docker pull caddy:latest

# Save to tar files
docker save matrixdotorg/synapse:latest > synapse.tar
docker save postgres:15 > postgres.tar
docker save vectorim/element-web:latest > element.tar
docker save caddy:latest > caddy.tar
```

Transfer tar files, then on offline machine:
```bash
docker load < synapse.tar
docker load < postgres.tar
docker load < element.tar
docker load < caddy.tar
```

### Client Apps to Pre-Download

| Platform | App | Download Source |
|----------|-----|-----------------|
| Linux | Element Desktop | .deb/.AppImage from element.io |
| Windows | Element Desktop | .exe from element.io |
| Android | Element | APK from F-Droid or element.io |
| iOS | Element | Can't sideload easily |
| Web | Element Web | Host vectorim/element-web container |

---

## Intranet-Only Setup (No Internet Ever)

### Addressing Options (No External Domain Needed)

| Method | Example | Matrix ID |
|--------|---------|-----------|
| **IP Address** | `192.168.1.100` | `@user:192.168.1.100` |
| **Hostname** | `matrix` | `@user:matrix` |
| **Local DNS** | `matrix.home` | `@user:matrix.home` |

**Note:** Matrix IDs are permanent. If you change the server name, all accounts break. Choose carefully.

### Option A: Direct IP Address

User IDs: `@alice:192.168.1.100`

**homeserver.yaml:**
```yaml
server_name: "192.168.1.100"
public_baseurl: "http://192.168.1.100:8008"
```

### Option B: Hostname via /etc/hosts

Add to `/etc/hosts` on all clients:
```
192.168.1.100   matrix
```

User IDs: `@alice:matrix`

**homeserver.yaml:**
```yaml
server_name: "matrix"
public_baseurl: "http://matrix:8008"
```

### Option C: Local DNS

Use dnsmasq/Pi-hole to resolve `matrix.home`:
```
address=/matrix.home/192.168.1.100
```

User IDs: `@alice:matrix.home`

### TLS for Intranet

**Option 1: No TLS (HTTP only)**

For trusted intranet, you can skip TLS:

```yaml
# homeserver.yaml
listeners:
  - port: 8008
    type: http
    tls: false
    resources:
      - names: [client, federation]
        compress: false
```

Element and other clients need to be configured to allow insecure connections.

**Option 2: Self-Signed Certificate**

```bash
openssl req -x509 -newkey rsa:4096 \
    -keyout /etc/matrix-synapse/matrix.key \
    -out /etc/matrix-synapse/matrix.crt \
    -days 3650 -nodes \
    -subj "/CN=matrix"
```

```yaml
# homeserver.yaml
listeners:
  - port: 8448
    type: http
    tls: true
    resources:
      - names: [client, federation]

tls_certificate_path: "/etc/matrix-synapse/matrix.crt"
tls_private_key_path: "/etc/matrix-synapse/matrix.key"
```

Clients will need to trust the self-signed cert.

### Disable Federation (Required for Intranet)

```yaml
# homeserver.yaml
federation_domain_whitelist: []
# Or simply don't open port 8448 externally
```

### Intranet Docker Compose Example

```yaml
version: '3'

services:
  synapse:
    image: matrixdotorg/synapse:latest
    environment:
      - SYNAPSE_SERVER_NAME=matrix
      - SYNAPSE_REPORT_STATS=no
    volumes:
      - ./synapse-data:/data
    ports:
      - "8008:8008"
    depends_on:
      - db

  db:
    image: postgres:15
    environment:
      - POSTGRES_USER=synapse
      - POSTGRES_PASSWORD=changeme
      - POSTGRES_DB=synapse
      - POSTGRES_INITDB_ARGS=--encoding=UTF-8 --lc-collate=C --lc-ctype=C
    volumes:
      - ./postgres-data:/var/lib/postgresql/data

  element:
    image: vectorim/element-web:latest
    volumes:
      - ./element-config.json:/app/config.json
    ports:
      - "8080:80"
```

**element-config.json for intranet:**
```json
{
    "default_server_config": {
        "m.homeserver": {
            "base_url": "http://matrix:8008",
            "server_name": "matrix"
        }
    },
    "disable_guests": true,
    "disable_3pid_login": true
}
```

### What Won't Work on Intranet

- Federation with any external Matrix servers
- Let's Encrypt certificates
- Push notifications (requires internet)
- Bridges to external services (Discord, Telegram, etc.)
- Login via Google/GitHub/etc.

### What Works on Intranet

- Local user accounts and messaging
- Rooms and spaces
- File uploads/sharing
- End-to-end encryption
- Voice/video calls (with local TURN)
- Element web client (self-hosted)

### Additional Note: Matrix is Overkill for Intranet

If you're running a closed intranet with no federation needs, Matrix's complexity provides little benefit over XMPP. Consider:

- XMPP uses ~25 MB RAM vs Matrix's 2-4 GB
- XMPP setup is simpler
- Matrix's main advantage (federation, bridges) is lost on intranet

---

## Sources
- [Synapse GitHub](https://github.com/matrix-org/synapse)
- [Matrix.org Hosting Guide](https://matrix.org/docs/older/understanding-synapse-hosting/)
- [DuckDNS](https://www.duckdns.org/)
