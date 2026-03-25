# Independent Communication

Research and setup guides for self-hosted, privacy-first communication systems that work on local networks without dependence on cloud services or big-tech ecosystems.

## Problem

When network access is limited or there is a risk of surveillance/spoofing, you need an end-to-end encrypted communication channel backed by servers you control, with support for peer-to-peer fallback.

## Key Requirements

- End-to-end encrypted text, voice, and file sharing
- Self-hosted on local hardware (Raspberry Pi, old laptop, etc.)
- Works on a LAN with no internet dependency
- No Google/Apple dependencies (F-Droid clients, no phone number registration)
- Video calls are a nice-to-have

## Top Recommendations

| Use Case | Solution |
|----------|----------|
| Lightweight, minimal resources | **XMPP / Prosody** (~25 MB RAM) |
| Full-featured chat with bridges | **Matrix / Synapse** (~350 MB+ RAM) |
| Voice communication | **TeamSpeak 3** (~60 MB RAM) |
| Video conferencing | **Jitsi Meet** (4 GB+ RAM) |

Both **Matrix** and **XMPP** score 7/7 on the core requirements. See the detailed comparison in [`readme.md`](readme.md).

## Setup Guides

- [`matrix-setup.md`](matrix-setup.md) -- Self-hosting Matrix/Synapse on a local PC
- [`xmpp-setup.md`](xmpp-setup.md) -- Self-hosting XMPP/Prosody on a local PC
- [`teamspeak-setup.md`](teamspeak-setup.md) -- Self-hosting TeamSpeak 3 on a local PC

## Detailed Research

The full platform comparison, requirements analysis, optimization conditions (censorship resistance, mesh networking, Tor integration), and deployment recommendations are in [`readme.md`](readme.md).
