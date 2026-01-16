# TeamSpeak Self-Hosting Plan (Local PC)

## Overview
TeamSpeak is voice communication software optimized for gaming and group voice chat. Unlike Matrix/XMPP (text-first), TeamSpeak is voice-first.

## Your Setup Context
- **Hosting**: Local shared desktop PC
- **IP**: Dynamic (needs DDNS)
- **Domain**: Free option preferred
- **Goal**: Family use → Community expansion

## How TeamSpeak Fits Your Setup

### 1. Resource Usage
- Very lightweight (~50-100 MB RAM)
- Minimal CPU usage
- **Verdict**: Won't impact your PC noticeably

### 2. Dynamic IP Handling
- Users connect via IP/hostname
- DDNS works well
- Clients can bookmark server address
- **Verdict**: Works fine with DDNS

### 3. Free Domain
- Connect via `yourname.duckdns.org`
- No account/identity tied to domain (unlike Matrix/XMPP)
- Easy to migrate later
- **Verdict**: No issues

### 4. Intermittent Uptime
- Voice chat only works when server is running
- No message queuing (it's real-time voice)
- Users see server as offline when PC is off
- **Verdict**: Acceptable for casual use

## Licensing

| License | Slots | Virtual Servers | Cost |
|---------|-------|-----------------|------|
| NPL (Non-Profit) | 32 | 1 | **Free** |
| Gamer | 64-512 | 2-10 | $50-200/year |
| Commercial | 512-1024 | 10+ | Contact sales |

**For family/small community**: Free NPL license (32 slots) is plenty.

## Hardware Requirements

| Resource | Impact |
|----------|--------|
| RAM | 50-100 MB (minimal) |
| CPU | Very low |
| Disk | < 500 MB |
| Network | Depends on users (~100 kbps per user) |

## Versions

### TeamSpeak 3 (Stable)
- Mature, well-documented
- Widely used
- Recommended for now

### TeamSpeak 6 (Beta)
- New version, still in development
- Docker-based deployment
- Not yet feature-complete for self-hosting
- Wait for stable release

## Ports to Forward

| Port | Protocol | Purpose |
|------|----------|---------|
| 9987 | UDP | Voice (required) |
| 10011 | TCP | ServerQuery (optional) |
| 30033 | TCP | File transfer (optional) |

**Minimum**: Just port 9987 UDP for basic voice chat.

## Installation Steps

### 1. Set Up DDNS

Same as XMPP plan - use DuckDNS:
```bash
mkdir -p ~/duckdns
cat > ~/duckdns/duck.sh << 'EOF'
#!/bin/bash
echo url="https://www.duckdns.org/update?domains=YOURDOMAIN&token=YOURTOKEN&ip=" | curl -k -o ~/duckdns/duck.log -K -
EOF
chmod +x ~/duckdns/duck.sh

# Add to crontab
(crontab -l 2>/dev/null; echo "*/5 * * * * ~/duckdns/duck.sh") | crontab -
```

### 2. Configure Router

Forward port 9987 UDP to your PC's local IP.

Optional (for file transfers and remote admin):
- 10011 TCP
- 30033 TCP

### 3. Download TeamSpeak Server

```bash
# Create directory
mkdir -p ~/teamspeak && cd ~/teamspeak

# Download latest TS3 server (Linux 64-bit)
wget https://files.teamspeak-services.com/releases/server/3.13.7/teamspeak3-server_linux_amd64-3.13.7.tar.bz2

# Extract
tar xjf teamspeak3-server_linux_amd64-3.13.7.tar.bz2
cd teamspeak3-server_linux_amd64
```

### 4. Accept License

```bash
# Create license acceptance file
touch .ts3server_license_accepted
```

### 5. First Run (Important!)

```bash
# Start server for first time
./ts3server_startscript.sh start

# IMPORTANT: Note the output! It contains:
# - Server Admin Token (privilege key)
# - ServerQuery admin password
# Save these somewhere safe!
```

Example output to save:
```
------------------------------------------------------------------
                      I M P O R T A N T
------------------------------------------------------------------
Server Query Admin Account created
loginname= "serveradmin", password= "aBcDeFgH"

------------------------------------------------------------------
ServerAdmin privilege key created, please use it to gain
serveradmin rights for your virtualserver. please
also check the doc/privilegekey_guide.txt for details.

token=ABCD1234xyz...
------------------------------------------------------------------
```

### 6. Connect and Claim Admin

1. Download TeamSpeak client on your device
2. Connect to: `yourname.duckdns.org`
3. Go to: Permissions → Use Privilege Key
4. Enter the token from step 5
5. You now have admin rights

### 7. Basic Configuration

In TeamSpeak client (as admin):

1. **Server name**: Right-click server → Edit Virtual Server
2. **Password** (optional): Set server password for privacy
3. **Create channels**: Right-click → Create Channel
   - General
   - Gaming
   - AFK

### 8. Create as Systemd Service (Auto-start)

```bash
sudo nano /etc/systemd/system/teamspeak.service
```

Content:
```ini
[Unit]
Description=TeamSpeak 3 Server
After=network.target

[Service]
Type=forking
User=YOUR_USERNAME
WorkingDirectory=/home/YOUR_USERNAME/teamspeak/teamspeak3-server_linux_amd64
ExecStart=/home/YOUR_USERNAME/teamspeak/teamspeak3-server_linux_amd64/ts3server_startscript.sh start
ExecStop=/home/YOUR_USERNAME/teamspeak/teamspeak3-server_linux_amd64/ts3server_startscript.sh stop
Restart=on-failure

[Install]
WantedBy=multi-user.target
```

Enable:
```bash
sudo systemctl daemon-reload
sudo systemctl enable teamspeak
sudo systemctl start teamspeak
```

## Server Management

```bash
# Start
./ts3server_startscript.sh start

# Stop
./ts3server_startscript.sh stop

# Status
./ts3server_startscript.sh status

# Or via systemd
sudo systemctl status teamspeak
```

## Growth Path: Family → Community

### Phase 1: Family (Current)
- Free NPL license (32 slots)
- Basic channels
- No password or simple password

### Phase 2: Friends/Gaming Group
- Create more channels
- Set up channel groups (permissions)
- Still within 32 slots

### Phase 3: Larger Community
When exceeding 32 users:

1. **Purchase Gamer license** ($50/year for 64 slots)
2. **Consider dedicated hardware**
   - Raspberry Pi 4 works
   - Or old laptop
3. **Get proper domain** (optional, for professionalism)

## Firewall Configuration

```bash
# UFW
sudo ufw allow 9987/udp
sudo ufw allow 10011/tcp  # optional
sudo ufw allow 30033/tcp  # optional
```

## Backup Strategy

```bash
#!/bin/bash
BACKUP_DIR=~/teamspeak-backups
TS_DIR=~/teamspeak/teamspeak3-server_linux_amd64

mkdir -p $BACKUP_DIR
cp $TS_DIR/ts3server.sqlitedb $BACKUP_DIR/
cp $TS_DIR/query_ip_allowlist.txt $BACKUP_DIR/ 2>/dev/null
cp $TS_DIR/query_ip_denylist.txt $BACKUP_DIR/ 2>/dev/null
```

Key file: `ts3server.sqlitedb` contains all server config, users, permissions.

## Troubleshooting

```bash
# Check if running
pgrep -a ts3server

# View logs
cat ~/teamspeak/teamspeak3-server_linux_amd64/logs/ts3server_*.log

# Test port accessibility (from another network)
# Use online port checker or ask friend to connect
```

## Clients

| Platform | App |
|----------|-----|
| Windows | TeamSpeak 3 Client |
| macOS | TeamSpeak 3 Client |
| Linux | TeamSpeak 3 Client |
| Android | TeamSpeak 3 (paid, ~$2) |
| iOS | TeamSpeak 3 (paid, ~$2) |

Download: https://teamspeak.com/en/downloads/

## Comparison with Alternatives

| Feature | TeamSpeak | Mumble | Discord |
|---------|-----------|--------|---------|
| Self-hosted | Yes | Yes | No |
| Free | 32 slots | Unlimited | Yes (hosted) |
| Voice quality | Excellent | Excellent | Good |
| Resource usage | Low | Very low | N/A |
| Text chat | Basic | Basic | Excellent |
| Privacy | Full control | Full control | None |

**Mumble** is worth considering as a fully open-source alternative with no slot limits.

---

## Offline Setup (No Internet During Installation)

TeamSpeak is simpler than Matrix/XMPP for offline setup - it's a single self-contained binary.

### Files to Download

On a machine WITH internet:

**Server Software:**
```bash
mkdir ~/teamspeak-offline && cd ~/teamspeak-offline

# Download TeamSpeak 3 Server (Linux 64-bit)
wget https://files.teamspeak-services.com/releases/server/3.13.7/teamspeak3-server_linux_amd64-3.13.7.tar.bz2

# Download Windows server (if needed)
wget https://files.teamspeak-services.com/releases/server/3.13.7/teamspeak3-server_win64-3.13.7.zip
```

**No additional packages needed** - TeamSpeak server is standalone.

### Client Apps to Pre-Download

| Platform | Download URL |
|----------|--------------|
| Windows | https://www.teamspeak.com/en/downloads/ |
| macOS | https://www.teamspeak.com/en/downloads/ |
| Linux | https://www.teamspeak.com/en/downloads/ |
| Android | APK from APKMirror or APKPure |
| iOS | Must download from App Store (requires internet) |

### Transfer and Install

1. Copy `teamspeak-offline` folder via USB/local network
2. Extract the archive:
   ```bash
   tar xjf teamspeak3-server_linux_amd64-3.13.7.tar.bz2
   cd teamspeak3-server_linux_amd64
   ```
3. Accept license: `touch .ts3server_license_accepted`
4. Run: `./ts3server_startscript.sh start`

That's it - no dependencies, no package manager needed.

---

## Intranet-Only Setup (No Internet Ever)

TeamSpeak works perfectly on isolated networks.

### Addressing Options

| Method | Connection Address | Notes |
|--------|-------------------|-------|
| **IP Address** | `192.168.1.100` | Simplest, works immediately |
| **Hostname** | `teamspeak` | Needs /etc/hosts or DNS |
| **Local DNS** | `voice.home` | Professional |
| **.local mDNS** | `teamspeak.local` | Auto-discovery (Linux/Mac) |

### Option A: Direct IP Address (Recommended)

Users connect to: `192.168.1.100`

**No configuration needed** - TeamSpeak binds to all interfaces by default.

```bash
./ts3server_startscript.sh start
# Server listens on 0.0.0.0:9987
```

Clients connect by adding new server with IP `192.168.1.100`.

### Option B: Hostname via /etc/hosts

Add to `/etc/hosts` on all client machines:
```
192.168.1.100   teamspeak
```

Users connect to: `teamspeak`

### Option C: Local DNS

Configure dnsmasq/Pi-hole:
```
address=/voice.home/192.168.1.100
```

Users connect to: `voice.home`

### Server Configuration for Intranet

TeamSpeak works out-of-the-box with no special intranet configuration.

**Optional: Customize server name**

Edit `ts3server.ini` (create if doesn't exist):
```ini
machine_id=
default_voice_port=9987
voice_ip=0.0.0.0
licensepath=
query_port=10011
query_ip=0.0.0.0
```

Or use environment variables:
```bash
export TS3SERVER_LICENSEPATH=
./ts3server_startscript.sh start
```

### License Activation on Intranet

**Important:** The free NPL (Non-Profit License) requires internet activation on first run.

**Workaround for offline/intranet:**

1. **Option 1: Activate elsewhere**
   - Run server temporarily on internet-connected machine
   - Activate NPL license (automatic on first run)
   - Copy the entire server directory (includes license)
   - Move to intranet server

2. **Option 2: Use without activation**
   - Without internet, TeamSpeak defaults to **32 slots** anyway
   - You won't see "NPL" branding, but functionality is identical
   - No activation needed for basic use

3. **Option 3: Paid license**
   - Purchase Gamer license from TeamSpeak
   - Receive `licensekey.dat` file
   - Place in server directory (no internet needed)

### TLS/Encryption

TeamSpeak handles encryption at the protocol level (TeamSpeak proprietary encryption), not via TLS certificates like XMPP/Matrix.

**No certificate setup needed** - encryption works automatically on intranet.

### Intranet-Specific Settings

**Disable automatic updates** (won't work anyway):
```bash
# No setting needed - updates are manual for Linux server
```

**Disable usage statistics:**
```bash
# Add to ts3server.ini
serveradmin_query_send_interval=0
```

### Client Configuration for Intranet

1. Open TeamSpeak client
2. Connections → Connect
3. Server Address: `192.168.1.100` (or hostname)
4. Server Password: (if you set one)
5. Nickname: Your name

**No special client configuration needed for intranet.**

### What Won't Work on Intranet

- Automatic public server listing
- Update checks
- MyTeamSpeak account integration
- Accounting/licensing server communication (use workaround above)
- Mobile push notifications

### What Works Fine on Intranet

- All voice communication
- Text chat
- File transfers
- Channels and permissions
- Voice quality/codecs
- All core features

### Complete Intranet Example

**Server setup:**
```bash
# Extract server
tar xjf teamspeak3-server_linux_amd64-3.13.7.tar.bz2
cd teamspeak3-server_linux_amd64

# Accept license
touch .ts3server_license_accepted

# Start server
./ts3server_startscript.sh start

# Save the admin token from output!
```

**Client setup:**
1. Install TeamSpeak client (from pre-downloaded installer)
2. Connect to server IP: `192.168.1.100`
3. Use privilege key from server startup
4. Done

**No internet required at any point.**

### Advantages Over XMPP/Matrix for Intranet

| Feature | TeamSpeak | XMPP | Matrix |
|---------|-----------|------|--------|
| Offline install | Single file | Many packages | Very complex |
| TLS setup | Not needed | Required | Required |
| Resource usage | ~50 MB | ~25 MB | ~2-4 GB |
| Configuration | None | Moderate | Extensive |
| Voice quality | Excellent | Needs TURN | Needs TURN |

**TeamSpeak is the easiest option for intranet voice chat.**

### mDNS Auto-Discovery (Advanced)

For user convenience, advertise server via mDNS:

**Install Avahi:**
```bash
sudo apt install avahi-daemon
```

**Create service file** `/etc/avahi/services/teamspeak.service`:
```xml
<?xml version="1.0" standalone='no'?>
<!DOCTYPE service-group SYSTEM "avahi-service.dtd">
<service-group>
  <name>TeamSpeak Server</name>
  <service>
    <type>_ts3._udp</type>
    <port>9987</port>
  </service>
</service-group>
```

Now clients on the local network can discover the server automatically.

---

## Sources
- [TeamSpeak Official Setup Guide](https://support.teamspeak.com/hc/en-us/articles/360002712958-How-do-I-set-up-my-own-non-commercial-TS3-server)
- [TeamSpeak Licensing](https://teamspeak.com/en/features/licensing/)
- [IONOS TeamSpeak Guide](https://www.ionos.com/digitalguide/server/know-how/teamspeak-server/)
- [Self-Host TeamSpeak Tutorial](https://akabenny.com/posts/how-to-self-host-a-teamspeak-server/)
- [TeamSpeak 6 Beta (GitHub)](https://github.com/teamspeak/teamspeak6-server)
