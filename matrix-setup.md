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

## Sources
- [Synapse GitHub](https://github.com/matrix-org/synapse)
- [Matrix.org Hosting Guide](https://matrix.org/docs/older/understanding-synapse-hosting/)
- [DuckDNS](https://www.duckdns.org/)
