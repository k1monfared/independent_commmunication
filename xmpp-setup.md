# XMPP Self-Hosting Plan (Local PC)

## Overview
XMPP is an open standard for messaging. Extremely lightweight - ideal for your setup.

## Your Setup Context
- **Hosting**: Local shared desktop PC
- **IP**: Dynamic (needs DDNS)
- **Domain**: Free option preferred
- **Goal**: Family use → Community expansion

## Why XMPP Fits Your Setup Well

### 1. Resource Usage
- Prosody uses ~25-50 MB RAM
- Negligible CPU usage
- **Verdict**: Won't impact your PC usage at all

### 2. Dynamic IP Handling
- XMPP handles reconnections gracefully
- Clients automatically reconnect
- Federation more tolerant of IP changes
- **Verdict**: Works reasonably well

### 3. Free Domain
- Works fine with DDNS subdomains
- JIDs look like: `user@yourname.duckdns.org`
- Can migrate to proper domain later
- **Verdict**: Perfectly usable

### 4. Intermittent Uptime
- XMPP designed for unreliable connections
- Messages queue on sender side
- Offline message storage available
- **Verdict**: Handles shared PC scenario well

## Free Domain Options

| Service | Example Domain | Notes |
|---------|---------------|-------|
| DuckDNS | `you.duckdns.org` | Recommended, simple |
| No-IP | `you.ddns.net` | Needs renewal every 30 days |
| FreeDNS | `you.afraid.org` | Many domain options |
| Dynu | `you.dynu.com` | Free, reliable |

**Recommended**: DuckDNS - simple, free, no renewal hassle

## Hardware Requirements (Your PC)

| Resource | Impact |
|----------|--------|
| RAM | 25-50 MB (negligible) |
| CPU | Almost none |
| Disk | < 1 GB |
| Network | Minimal |

## Server Choice: Prosody

For your setup, Prosody is the best choice:
- Simpler configuration
- Lower resources than ejabberd
- Easy to install on any Linux

## Installation Steps

### 1. Set Up DDNS

**DuckDNS Setup:**
```bash
# 1. Go to duckdns.org and sign in
# 2. Create a subdomain (e.g., "myfamily")
# 3. Note your token

# Install updater
mkdir -p ~/duckdns
cat > ~/duckdns/duck.sh << 'EOF'
#!/bin/bash
echo url="https://www.duckdns.org/update?domains=YOURDOMAIN&token=YOURTOKEN&ip=" | curl -k -o ~/duckdns/duck.log -K -
EOF
chmod +x ~/duckdns/duck.sh

# Add to crontab (update every 5 min)
(crontab -l 2>/dev/null; echo "*/5 * * * * ~/duckdns/duck.sh") | crontab -
```

### 2. Configure Router

Forward these ports to your PC's local IP:

| Port | Protocol | Purpose |
|------|----------|---------|
| 5222 | TCP | Client connections |
| 5269 | TCP | Server-to-server (federation) |
| 5280 | TCP | Web admin (optional) |

### 3. Install Prosody

```bash
# Debian/Ubuntu
sudo apt update
sudo apt install prosody

# Install community modules (recommended)
sudo apt install mercurial
sudo hg clone https://hg.prosody.im/prosody-modules/ /opt/prosody-modules
```

### 4. Get TLS Certificate

```bash
# Install certbot
sudo apt install certbot

# Get certificate (stop any web server first)
sudo certbot certonly --standalone -d yourname.duckdns.org

# Certificates will be at:
# /etc/letsencrypt/live/yourname.duckdns.org/fullchain.pem
# /etc/letsencrypt/live/yourname.duckdns.org/privkey.pem
```

### 5. Configure Prosody

Edit `/etc/prosody/prosody.cfg.lua`:

```lua
-- Basic settings
admins = { "admin@yourname.duckdns.org" }

-- Enable modules
modules_enabled = {
    "roster";
    "saslauth";
    "tls";
    "dialback";
    "disco";
    "carbons";
    "pep";
    "private";
    "blocklist";
    "vcard4";
    "vcard_legacy";
    "version";
    "uptime";
    "time";
    "ping";
    "register";
    "mam";  -- Message archive
    "csi";  -- Mobile optimization
    "smacks";  -- Stream management (reconnection)
}

-- TLS settings
ssl = {
    certificate = "/etc/letsencrypt/live/yourname.duckdns.org/fullchain.pem";
    key = "/etc/letsencrypt/live/yourname.duckdns.org/privkey.pem";
}

-- Your virtual host
VirtualHost "yourname.duckdns.org"

-- Disable open registration (add users manually)
allow_registration = false

-- For community phase later, add:
-- Component "conference.yourname.duckdns.org" "muc"
```

### 6. Import Certificates

```bash
sudo prosodyctl --root cert import /etc/letsencrypt/live
```

### 7. Start and Enable

```bash
sudo systemctl enable prosody
sudo systemctl start prosody
sudo systemctl status prosody
```

### 8. Create User Accounts

```bash
sudo prosodyctl adduser admin@yourname.duckdns.org
sudo prosodyctl adduser familymember@yourname.duckdns.org
```

### 9. Test Connection

Install a client and connect:
- **Android**: Conversations (paid) or Blabber.im (free)
- **iOS**: Siskin IM or Monal
- **Desktop**: Gajim (cross-platform) or Dino (Linux)

## Growth Path: Family → Community

### Phase 1: Family (Current Plan)
- Basic Prosody setup
- Manual user registration
- No group chats needed

### Phase 2: Extended Family/Friends
Add group chat support:
```lua
-- Add to prosody.cfg.lua
Component "conference.yourname.duckdns.org" "muc"
    modules_enabled = { "muc_mam" }
```

### Phase 3: Community
When ready to scale:

1. **Get proper domain** (~$10-15/year)
   - Migrate users or start fresh
   - More professional appearance

2. **Consider dedicated hardware**
   - Raspberry Pi 4 (~$50-80) is plenty
   - Always-on, low power (5W)

3. **Enable moderated registration**
   ```lua
   allow_registration = true
   registration_invite_only = true  -- Require invite links
   ```

4. **Add voice/video** (optional)
   - Install Coturn for TURN server
   - More complex, needs additional ports

## Firewall Configuration

If using UFW:
```bash
sudo ufw allow 5222/tcp
sudo ufw allow 5269/tcp
sudo ufw allow 5280/tcp  # optional, for web admin
```

## Automatic Startup (Shared PC)

Prosody starts automatically on boot. If you want it to only run when you're logged in:

```bash
# Disable auto-start
sudo systemctl disable prosody

# Start manually when needed
sudo systemctl start prosody
```

## Backup Strategy

```bash
# Backup script
#!/bin/bash
BACKUP_DIR=~/xmpp-backups
mkdir -p $BACKUP_DIR
cp /etc/prosody/prosody.cfg.lua $BACKUP_DIR/
cp -r /var/lib/prosody $BACKUP_DIR/data/
```

## Troubleshooting

```bash
# Check status
sudo systemctl status prosody

# View logs
sudo journalctl -u prosody -f

# Test configuration
sudo prosodyctl check config

# Check connectivity
sudo prosodyctl check dns
```

## Recommended Clients

| Platform | App | Notes |
|----------|-----|-------|
| Android | Conversations | Best, $3 or free via F-Droid |
| Android | Blabber.im | Free, Conversations fork |
| iOS | Siskin IM | Good, free |
| iOS | Monal | Good, free |
| Linux | Dino | Modern, clean |
| Windows/Mac/Linux | Gajim | Feature-rich |
| Web | Converse.js | Self-hostable |

## Sources
- [Prosody Documentation](https://prosody.im/doc/)
- [JoinJabber Self-Hosting Guide](https://joinjabber.org/tutorials/self-hosted/)
- [Prosody Setup Tutorial 2025](https://voxelmanip.se/2025/06/25/setting-up-an-xmpp-server-with-prosody/)
- [DuckDNS](https://www.duckdns.org/)
