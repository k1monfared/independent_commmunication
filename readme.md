# Independent Communication Solutions

## Issue
- In cases where there is limited network access and/or a concern of spoofing where communication is limited

## Needs
- An end-to-end encrypted communication channel with independent servers to be run locally and allowance for P2P connections
- **Priority:** Text and voice with file sharing (image/video), video call is a plus

---

# Requirements Summary

## My Requirements

| Requirement | Priority | Notes |
|-------------|----------|-------|
| Text chat | **Must have** | Full-featured, not basic |
| Voice calls | **Must have** | Quality matters |
| File sharing (image/video) | **Must have** | Rich media support |
| Video calls | Nice to have | Plus if available |
| Works offline/locally | **Must have** | No internet dependency |
| No Google/Apple dependencies | **Must have** | F-Droid apps, no phone# |
| Self-hostable | **Must have** | Full control |

## Satisfaction Conditions (Must Have)

| ID | Category | Key Requirements |
|----|----------|------------------|
| S1 | Hardware | Local server (RPi4+), router, storage (32GB+), UPS |
| S2 | Network | LAN, static IP, no external dependencies |
| S3 | Software | Server (Matrix/XMPP), database, TLS, F-Droid clients |
| S4 | Security | E2E encryption, no phone#, local auth, physical security |
| S5 | Operations | Offline install packages, local docs, backup plan |

## Optimization Conditions (Nice to Have)

| ID | Category | Key Enhancements |
|----|----------|------------------|
| O1 | Censorship Resistance | V2Ray, Shadowsocks, domain fronting, ECH |
| O2 | Network Resilience | Mesh (Meshtastic/Bridgefy), federation, P2P fallback |
| O3 | Usability | Modern UI, voice/video, file sharing, multi-device |
| O4 | Advanced Security | Disappearing messages, Tor, disk encryption |
| O5 | Operations | Monitoring, automated backups, incident response |

---

# Comparison Tables

## Main Platform Comparison

| Feature | Signal | Matrix | TeamSpeak 3 | XMPP/Prosody | Jitsi Meet |
|---------|--------|--------|-------------|--------------|------------|
| **Self-Host Ready** | ❌ Difficult | ✅ Yes | ✅ Yes | ✅ Yes | ✅ Yes |
| **RAM Usage** | Cloud-dependent | ~350MB+ | ~60-70MB | ~25MB | 4GB+ |
| **Setup Difficulty** | Hard | Moderate | Easy | Easy | Moderate |
| **E2E Encryption** | ✅ Signal Protocol | ✅ Olm/Megolm | ✅ AES | ✅ OMEMO | ✅ WebRTC |
| **Federation** | ❌ No | ✅ Yes | ❌ No | ✅ Yes | ❌ No |
| **Text Chat** | ✅ Full | ✅ Full | ⚠️ Basic | ✅ Full | ❌ No |
| **Voice Calls** | ✅ Yes | ✅ Yes | ✅ Excellent | ✅ Via TURN | ✅ Yes |
| **Video Calls** | ✅ Yes | ✅ Yes | ❌ No | ✅ Via TURN | ✅ Excellent |
| **File Sharing** | ✅ Yes | ✅ Yes | ❌ No | ✅ Yes | ❌ No |
| **Image/Video Share** | ✅ Yes | ✅ Yes | ❌ No | ✅ Yes | ❌ Screen only |
| **Works Offline/Local** | ❌ No | ✅ Yes | ✅ Yes | ✅ Yes | ✅ Yes |
| **No Google Dependencies** | ❌ Needs phone# | ✅ F-Droid | ✅ Yes | ✅ F-Droid | ✅ Yes |
| **License** | AGPL | Apache 2.0 | Proprietary | MIT | Apache 2.0 |

## My Requirements Match

| Requirement | Signal | Matrix | TeamSpeak 3 | XMPP/Prosody | Jitsi Meet |
|-------------|--------|--------|-------------|--------------|------------|
| Text chat (full) | ✅ | ✅ | ⚠️ Basic | ✅ | ❌ |
| Voice calls | ✅ | ✅ | ✅ Excellent | ✅ | ✅ |
| File sharing | ✅ | ✅ | ❌ | ✅ | ❌ |
| Video calls | ✅ | ✅ | ❌ | ✅ | ✅ Excellent |
| Works offline/local | ❌ | ✅ | ✅ | ✅ | ✅ |
| No Google deps | ❌ | ✅ | ✅ | ✅ | ✅ |
| Self-hostable | ❌ Hard | ✅ | ✅ | ✅ | ✅ |
| **TOTAL MATCH** | ❌ 4/7 | ✅ **7/7** | ⚠️ 4/7 | ✅ **7/7** | ⚠️ 4/7 |

## Satisfaction Conditions Match

| Condition | Signal | Matrix | TeamSpeak 3 | XMPP/Prosody | Jitsi Meet |
|-----------|--------|--------|-------------|--------------|------------|
| S1: Runs on RPi4/laptop | ❌ Cloud | ✅ Yes | ✅ Yes | ✅ Yes | ⚠️ 4GB+ |
| S2: Works on LAN only | ❌ No | ✅ Yes | ✅ Yes | ✅ Yes | ✅ Yes |
| S3: F-Droid client | ❌ No | ✅ Element | ✅ Yes | ✅ Conversations | ✅ Yes |
| S4: E2E + no phone# | ❌ Needs phone | ✅ Yes | ✅ Yes | ✅ Yes | ✅ Yes |
| S5: Offline installable | ❌ Cloud deps | ✅ Yes | ✅ Yes | ✅ Yes | ✅ Yes |
| **SATISFIES ALL** | ❌ **No** | ✅ **Yes** | ✅ **Yes** | ✅ **Yes** | ⚠️ **Mostly** |

## Optimization Conditions Match

| Condition | Signal | Matrix | TeamSpeak 3 | XMPP/Prosody | Jitsi Meet |
|-----------|--------|--------|-------------|--------------|------------|
| O1: Traffic obfuscation | ❌ | ⚠️ Possible | ⚠️ Possible | ⚠️ Possible | ⚠️ Possible |
| O2: Mesh/P2P fallback | ❌ | ⚠️ Federation | ❌ | ✅ Federation | ❌ |
| O3: Modern UI + features | ✅ | ✅ Element | ⚠️ Voice only | ⚠️ Varied | ✅ |
| O4: Disappearing msgs | ✅ | ✅ | ❌ | ✅ | ❌ |
| O5: Easy backup/export | ❌ | ✅ | ✅ | ✅ | ⚠️ |
| **OPTIMIZATION SCORE** | ⚠️ 2/5 | ✅ **4/5** | ⚠️ 2/5 | ✅ **4/5** | ⚠️ 2/5 |

## Final Recommendation

| Solution | Text | Voice | Files | Video | Local | No Google | S-Conditions | O-Conditions | **Verdict** |
|----------|------|-------|-------|-------|-------|-----------|--------------|--------------|-------------|
| **Matrix** | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ 5/5 | ✅ 4/5 | ⭐⭐⭐ **BEST** |
| **XMPP/Prosody** | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ 5/5 | ✅ 4/5 | ⭐⭐⭐ **BEST (lightweight)** |
| **Signal** | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ 0/5 | ⚠️ 2/5 | ❌ Can't self-host |
| **Jitsi Meet** | ❌ | ✅ | ❌ | ✅ | ✅ | ✅ | ⚠️ 4/5 | ⚠️ 2/5 | ⚠️ Video only |
| **TeamSpeak 3** | ⚠️ | ✅ | ❌ | ❌ | ✅ | ✅ | ✅ 5/5 | ⚠️ 2/5 | ⚠️ Voice only |

## Recommendation by Use Case

| Use Case | Recommended Solution |
|----------|---------------------|
| Limited resources / minimal hardware | XMPP/Prosody |
| Full-featured chat with bridges | Matrix/Synapse |
| Voice communication / gaming | TeamSpeak 3 |
| Video conferencing | Jitsi Meet |
| Maximum privacy (text) | XMPP with OMEMO or Matrix with E2EE |
| P2P focus | XMPP (with TURN) or Matrix |
| **Censored region (text+voice+files+video)** | **Matrix** or **XMPP + Jitsi** |
| **Emergency/no network** | **Meshtastic** or **Briar** |

---

# Requirements Detail

## S1. Hardware Infrastructure

| Requirement | Details | Why Critical |
|-------------|---------|--------------|
| **Local Server Device** | Raspberry Pi 4+, old laptop, or small PC | Runs communication services locally |
| **Network Equipment** | Router/switch for LAN | Creates isolated network |
| **Storage** | SD card or SSD (32GB+ recommended) | Stores messages, files, database |
| **Power Supply** | Reliable power, consider UPS | Server must stay running |
| **User Devices** | Phones/laptops for each user | Access the communication system |

## S2. Network Configuration

| Requirement | Details | Why Critical |
|-------------|---------|--------------|
| **Local Area Network (LAN)** | Wired (Ethernet) or WiFi | Core communication path |
| **Static IP for Server** | e.g., 192.168.1.100 | Clients need consistent endpoint |
| **Local DNS (optional)** | Or use IP directly | Avoid relying on external DNS |
| **No External Dependencies** | No cloud services, no Google/Apple push | System must work when isolated |

## S3. Software Stack

| Requirement | Details | Why Critical |
|-------------|---------|--------------|
| **Server Software** | Matrix (Synapse/Dendrite) or XMPP (Prosody) | Core messaging service |
| **Database** | PostgreSQL (Matrix) or SQLite (XMPP) | Message storage |
| **TLS Certificates** | Self-signed is OK for local network | Encryption in transit |
| **Client Apps** | F-Droid version (no Google dependencies) | Must work without Play Services |

## S4. Security Baseline

| Requirement | Details | Why Critical |
|-------------|---------|--------------|
| **End-to-End Encryption** | OMEMO (XMPP) or Olm/Megolm (Matrix) | Messages unreadable even if server seized |
| **No Phone Number Registration** | Use usernames instead | Privacy, no identity linking |
| **Local Authentication** | Server-controlled user accounts | No external auth providers |
| **Physical Security** | Server in secure location | Prevent seizure/tampering |

## S5. Operational Baseline

| Requirement | Details | Why Critical |
|-------------|---------|--------------|
| **Offline Installation** | Pre-download all packages | Can't rely on internet during setup |
| **Documentation** | Setup guide in local language | Users must be able to troubleshoot |
| **Backup Plan** | Data export capability | Recover if hardware fails |

---

# Optimizations Detail

## O1. Censorship Resistance (If Internet Available)

| Enhancement | Details | Benefit |
|-------------|---------|---------|
| **Traffic Obfuscation** | V2Ray, Shadowsocks, Outline | Makes traffic look like normal HTTPS |
| **Domain Fronting** | Route through CDNs | Hide destination from DPI |
| **ECH (Encrypted Client Hello)** | Hide SNI from censors | Bypass SNI-based blocking |
| **Rotating IPs/Domains** | Dynamic DNS, multiple endpoints | Resist IP blocking |
| **Bridge Servers** | Intermediate nodes outside censored region | Relay traffic if direct blocked |

## O2. Network Resilience

| Enhancement | Details | Benefit |
|-------------|---------|---------|
| **Mesh Networking** | Meshtastic (LoRa) or Bridgefy (Bluetooth) | Works even without any network |
| **Multiple Servers** | Federated setup | No single point of failure |
| **Offline Message Queue** | Store-and-forward | Messages delivered when reconnected |
| **Peer-to-Peer Mode** | Direct device communication | Works if server down |

## O3. Usability Improvements

| Enhancement | Details | Benefit |
|-------------|---------|---------|
| **Element/SchildiChat** | Familiar modern UI | Lower learning curve |
| **Voice/Video (Jitsi)** | Add video conferencing | Complete communication solution |
| **File Sharing** | Image/video support | Rich media communication |
| **Multi-Device Sync** | Cross-device message access | Convenience |
| **Group Chats** | Rooms/channels | Team communication |

## O4. Advanced Security

| Enhancement | Details | Benefit |
|-------------|---------|---------|
| **Disappearing Messages** | Auto-delete after time | Reduce evidence if seized |
| **Plausible Deniability** | Hidden/steganographic modes | Conceal app's true purpose |
| **Tor/I2P Integration** | Anonymize traffic | Hide who is communicating |
| **Hardware Security Keys** | YubiKey, etc. | Stronger authentication |
| **Full Disk Encryption** | On server device | Protect data at rest |

## O5. Operational Excellence

| Enhancement | Details | Benefit |
|-------------|---------|---------|
| **Monitoring/Alerts** | Server health checks | Know when issues arise |
| **Automated Backups** | Scheduled data export | Data protection |
| **User Training** | Security awareness | Reduce human error |
| **Incident Response Plan** | What to do if compromised | Minimize damage |

---

# Mesh Networking Backup Layer

For when all else fails - no internet, no local network, just devices.

## Meshtastic (LoRa-based)

| Aspect | Details |
|--------|---------|
| **Range** | 1-10+ km depending on terrain |
| **Hardware** | LILYGO T-Beam, Heltec LoRa32, RAK WisBlock (~$25-50 each) |
| **Power** | Week+ battery life, solar possible |
| **Encryption** | AES256 |
| **Setup** | Flash firmware, pair via Bluetooth to phone app |
| **Limitations** | Text only, ~200 chars/message, low bandwidth |
| **Best for** | Emergency coordination, rural areas, protests |

## Bridgefy (Bluetooth-based)

| Aspect | Details |
|--------|---------|
| **Range** | ~100m direct, multi-hop extends range |
| **Hardware** | Any smartphone with Bluetooth |
| **Encryption** | Signal Protocol |
| **Setup** | Install app, enable Bluetooth |
| **Limitations** | Requires density of users, battery drain |
| **Best for** | Urban protests, temporary gatherings |

## Briar (Tor + Bluetooth + WiFi)

| Aspect | Details |
|--------|---------|
| **Modes** | Internet (via Tor), WiFi direct, Bluetooth |
| **Encryption** | End-to-end, forward secrecy |
| **Setup** | Install from F-Droid |
| **Limitations** | Android only, no iOS |
| **Best for** | Activists, journalists, high-risk users |

---

# Censorship Circumvention

Full technical details for bypassing network censorship.

## V2Ray/Xray

**Architecture:**
```
[Client] → [V2Ray Client] → [Obfuscated Traffic] → [V2Ray Server] → [Internet]
```

**Protocols:**
| Protocol | Description |
|----------|-------------|
| VMess | V2Ray native protocol, encrypted |
| VLESS | Lighter version, less overhead |
| Trojan | Mimics HTTPS traffic perfectly |
| XTLS | Hardware-accelerated TLS, better performance |

**Recommended:** VLESS + XTLS + Reality (most resistant to detection as of 2025)

**Server Requirements:**
- VPS outside censored region ($5-10/month)
- Install: `bash <(curl -L https://raw.githubusercontent.com/XTLS/Xray-install/main/install-release.sh)`

## Shadowsocks-rust

**Modern Setup:**
- Use shadowsocks-rust (faster than original)
- Add v2ray-plugin for WebSocket obfuscation
- Use AEAD ciphers: `chacha20-ietf-poly1305` or `aes-256-gcm`

**Example Command:**
```bash
ssserver -s 0.0.0.0:443 -k "password" -m aes-256-gcm \
  --plugin v2ray-plugin \
  --plugin-opts "server;tls;host=legitimate-site.com"
```

## Domain Fronting

**Concept:**
- Outer TLS SNI shows: `allowed-cdn.com` (Cloudflare, Azure, etc.)
- Inner HTTP Host header shows: `your-actual-server.com`
- Censor sees only allowed CDN traffic

**Status (2025):**
| Provider | Status |
|----------|--------|
| Google | Disabled |
| Amazon | Disabled |
| Cloudflare | Limited (requires business plan) |
| Azure | Still works in some configurations |
| Fastly | Still works |

**Alternative - ECH (Encrypted Client Hello):**
- SNI encrypted, censor can't see destination
- Firefox: enabled by default
- Chrome: enable via flags

## Outline VPN (For Non-Technical Users)

**Why Outline:**
- One-click server setup (DigitalOcean, AWS, etc.)
- Automatic Shadowsocks configuration
- Simple key sharing via link
- Maintained by Jigsaw (Google subsidiary)

**Setup:**
1. Install Outline Manager (desktop)
2. Create server on cloud provider
3. Share access keys with users
4. Users install Outline Client

## Detection Avoidance Tips

| DO | DON'T |
|----|-------|
| Use ports 443, 8443 (HTTPS ports) | Use obvious ports (1080, 8388, etc.) |
| Enable TLS 1.3 with ALPN | Leave default configurations |
| Use REALITY or domain fronting | Share server with too many users |
| Randomize packet sizes | Use same server for VPN and other services |
| Limit concurrent connections | |

---

# Warnings

## Legal Risks

- Using circumvention tools may be illegal in some regions
- VPNs exist in legal gray area in China
- **November 2025:** China's Ministry of State Security issued warnings about VPN illegality
- Users must understand and accept risks

## Detection Risks

- China's GFW uses **machine learning** to detect obfuscated traffic
- Deep Packet Inspection analyzes **traffic patterns**, not just content
- Regional firewalls (e.g., Henan) are even more aggressive
- Active probing can identify circumvention tools

## Operational Security

- Physical security of server is paramount
- Trust all users - one compromised device exposes others
- Have a plan for device seizure scenarios
- Consider "duress" features (panic wipe)

---

# Recommended Architecture

## Minimum Viable Setup

Satisfies all S conditions with lowest resource usage.

```
[Raspberry Pi 4 / Old Laptop]
    ├── Prosody XMPP Server (~25MB RAM)
    ├── TURN Server (coturn) for voice/video
    └── Self-signed TLS certificate

[Local WiFi Router]
    └── Creates isolated network (no internet required)

[User Devices]
    └── Conversations (Android/F-Droid) or Dino (Desktop)
```

## Enhanced Setup

Adds optimization layers for better experience and resilience.

```
[Server Hardware]
    ├── Matrix/Synapse (more features) OR Prosody
    ├── Jitsi Meet (video conferencing)
    ├── PostgreSQL (for Matrix)
    └── coturn (TURN server)

[Censorship Resistance Layer]
    ├── V2Ray/Shadowsocks (if internet available)
    ├── Domain fronting configuration
    └── Multiple bridge servers (outside region)

[Mesh Backup]
    └── Meshtastic nodes (LoRa radios) for emergency
```

---

# Platform Details

## Signal Servers
**Ease of setup:** Difficult | **Self-host:** ❌ Not recommended

**Pros:** Strong E2E encryption, open source, good mobile apps

**Cons:** Cloud-dependent, requires phone#, no federation, complex setup (Redis, Java 24), no bridges/bots

**Sources:** [SoftwareMill](https://softwaremill.com/can-you-self-host-the-signal-server/)

---

## Matrix Based Servers
**Ease of setup:** Moderate | **Self-host:** ✅ Recommended

**Pros:** Federated, full data ownership, bridges (Slack, Discord, IRC), E2E encryption, Docker Compose works, ~350MB RAM

**Cons:** Requires PostgreSQL, can be resource-heavy, no built-in admin panel, metadata leaks in federation

**Clients:** Element, SchildiChat (F-Droid)

**Sources:** [Self-hosting Matrix 2025](https://blog.klein.ruhr/self-hosting-matrix-in-2025)

---

## TeamSpeak Servers
**Ease of setup:** Easy | **Self-host:** ✅ Yes

**Pros:** AES encryption, lightweight (60-70MB), excellent audio quality, flexible permissions

**Cons:** Voice-only, no file sharing, no video, paid tiers >32 users, no federation

**Sources:** [Zap-Hosting](https://zap-hosting.com/en/blog/2025/01/teamspeak-is-back-new-features-new-design-and-much-more/)

---

## XMPP/Prosody Servers
**Ease of setup:** Easy | **Self-host:** ✅ Recommended

**Pros:** Extremely lightweight (~25MB), federated, OMEMO E2E, 20+ years mature, MIT license

**Cons:** Fragmented clients, less polished UX, needs TURN for voice/video

**Clients:** Conversations (Android), Gajim/Dino (Desktop)

**Sources:** [Prosody IM](https://prosody.im/)

---

## Jitsi Meet Servers
**Ease of setup:** Moderate | **Self-host:** ✅ Yes

**Pros:** WebRTC video, no account needed, E2E available, browser-based, scalable

**Cons:** Resource intensive (4GB+ RAM), recording needs 8-12GB/instance, no persistent chat

**Sources:** [Jitsi Handbook](https://jitsi.github.io/handbook/docs/devops-guide/)

---

# Next Steps

1. ✅ Decide primary communication type needed (text, voice, video, or all)
   - **Answer:** Text and voice with file sharing (image/video), video call is a plus
2. ⬜ Evaluate available hardware resources
3. ⬜ Choose solution based on comparison above
4. ⬜ Plan network configuration (ports, domain, TLS)
5. ⬜ Gather offline installation packages before deployment
6. ⬜ Set up mesh backup for emergencies

---

# Sources

## Censorship Research
- [GFW Report - QUIC Censorship](https://gfw.report/publications/usenixsecurity25/en/)
- [Obfuscation Strategies Against GFW](https://arxiv.org/html/2503.02018v1)
- [How GFW Detects Encrypted Traffic](https://gfw.report/publications/usenixsecurity23/en/)
- [Regional Censorship in China](https://gfw.report/publications/sp25/en/)

## Mesh Networking
- [Bridgefy](https://bridgefy.me/)
- [Meshtastic Explained](https://cioprime.com/meshtastic-explained-the-best-off-grid-communication-and-mesh-network-tool-for-2025/)
- [RSF Mesh Messaging Guide](https://safety.rsf.org/need-to-communicate-while-offline-try-a-mesh-messaging-app/)

## Degoogled Apps
- [Element on F-Droid](https://f-droid.org/en/packages/im.vector.app/)
- [SchildiChat](https://schildi.chat/android/)

## Self-Hosting Guides
- [Self-hosting Matrix in 2025](https://blog.klein.ruhr/self-hosting-matrix-in-2025)
- [Prosody IM](https://prosody.im/)
- [Jitsi Self-Hosting Guide](https://jitsi.github.io/handbook/docs/devops-guide/)
