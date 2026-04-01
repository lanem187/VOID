<div align="center">

<img src="https://img.shields.io/badge/VOID-v3.0-6C63FF?style=for-the-badge&labelColor=0A0A0F&color=6C63FF" alt="VOID v3.0"/>

# ◌ VOID

### Zero-Knowledge P2P Secure Messaging

**No server. No phone number. No trace.**

[![Live App](https://img.shields.io/badge/▶_Open_VOID-Live_App-6C63FF?style=for-the-badge&labelColor=0A0A0F)](https://lanem187.github.io/VOID/)
[![Security](https://img.shields.io/badge/Crypto-AES--256--GCM_·_X3DH_·_Double_Ratchet-44FFAA?style=for-the-badge&labelColor=0A0A0F)](https://lanem187.github.io/VOID/)
[![Zero Server](https://img.shields.io/badge/Server-Zero_Owned-FF4466?style=for-the-badge&labelColor=0A0A0F)](https://lanem187.github.io/VOID/)
[![Tor](https://img.shields.io/badge/Tor-Compatible-7D4698?style=for-the-badge&labelColor=0A0A0F)](https://lanem187.github.io/VOID/)

</div>

---

## What is VOID?

VOID is a **serverless, zero-knowledge, end-to-end encrypted** messaging platform that runs entirely in your browser — no install, no account, no phone number. Messages travel directly **browser-to-browser via WebRTC**, with Nostr relays as an encrypted async fallback. The relay sees only AES-256-GCM ciphertext. Nobody — not even us — can read your messages.

It is a single `index.html` file. That is the entire application.

---

## How VOID Compares

| Feature | Signal | WhatsApp | Telegram | Session | ◌ VOID |
|---------|:------:|:--------:|:--------:|:-------:|:------:|
| Phone number needed | 😬 Yes | 😬 Yes | 😬 Yes | 😬 Yes | 🟢 Never |
| Tied to central server | 😬 Yes | 😬 Yes | 😬 Yes | 😬 Yes | 🟢 Never |
| Server sees who you talk to | 😬 Yes | 😬 Yes | 😬 Yes | 😬 Partial | 🟢 Never |
| All messages go through a relay | 😬 Always | 😬 Always | 😬 Always | 😬 Always | 🟢 P2P Direct |
| Panic wipe button | 😬 No | 😬 No | 😬 No | 😬 No | 🟢 Yes |
| Decoy account under duress | 😬 No | 😬 No | 😬 No | 😬 No | 🟢 Yes |
| Tor Browser support | 😬 Partial | 😬 No | 😬 No | 😬 No | 🟢 Full |
| Multi-relay onion encryption | 😬 No | 😬 No | 😬 No | 😬 No | 🟢 Yes |
| Steganography (hide msgs in images) | 😬 No | 😬 No | 😬 No | 😬 No | 🟢 Yes |
| Fully open — single auditable file | 😬 No | 😬 No | 😬 No | 😬 Partial | 🟢 Yes |
| Zero monthly cost | 🟢 Free | 🟢 Free | 🟢 Free | 🟢 Free | 🟢 Free |

> **VOID is the only messenger with all of these 🟢. Signal comes closest on encryption — but Signal still requires your phone number, knows who you talk to, and when.**

---

## Architecture

```
Alice's Browser                                        Bob's Browser
──────────────────                                  ──────────────────
  X3DH Key Agreement ──────── one time ───────────  X3DH Key Agreement
  Double Ratchet State                               Double Ratchet State
  AES-256-GCM encrypt                               AES-256-GCM decrypt
         │                                                  ▲
         │         ┌─────────────────────────┐             │
         └────────►│   WebRTC DataChannel    │─────────────┘
    (both online)  │   Direct P2P — zero     │
                   │   relay, zero metadata  │
                   └─────────────────────────┘

         │    ┌──────────┐  ┌──────────┐  ┌──────────┐    │
         └───►│ Relay 1  │  │ Relay 2  │  │ Relay 3  │────┘
  (async)     │ key A    │  │ key B    │  │ key C    │
              │ topic X  │  │ topic Y  │  │ topic Z  │
              └──────────┘  └──────────┘  └──────────┘
         Each relay gets a DIFFERENT ciphertext + DIFFERENT topic.
         No relay can link to any other relay's message.
```

---

## Cryptography

```
Key Exchange:     X3DH (Extended Triple Diffie-Hellman) — Signal Protocol
Forward Secrecy:  Double Ratchet — per-message key rotation
Message Crypto:   AES-256-GCM — 256-bit key, 128-bit auth tag, 12-byte IV
Key Derivation:   HKDF-SHA256
Identity Signing: ECDSA P-256 (SPK signature verification)
Passphrase KDF:   PBKDF2-SHA256 — 600,000 iterations
Nostr Signing:    secp256k1 + BIP340 Schnorr
Runtime:          WebCrypto API — native browser, zero libraries
```

Every message uses a **unique encryption key**. Compromise one message — get nothing else.

---

## Features

**Core Security**
- 🔐 Double Ratchet + X3DH — same cryptographic protocol as Signal
- 🔑 Per-message forward secrecy — every message uses a unique key, past messages safe forever
- 🧅 Tor Browser compatible — works natively, zero configuration
- ☠ Panic wipe — hold 3 seconds, everything destroyed instantly
- 🪬 Decoy account — dual passphrase, completely separate empty account under duress

**Multi-Relay Onion Encryption**
- 🔀 Each relay gets a **different ciphertext** encrypted with its own ephemeral key
- 🔀 Each relay gets a **different topic hash** — cannot correlate across relays
- 🔀 Timestamps jittered ±8 seconds — timing correlation broken
- 🔀 Increasing random delays per relay — traffic pattern hidden
- 🔀 Fixed 4KB packet padding — message size hidden
- 🔀 Dummy traffic every ~10s — active/idle pattern hidden

**Messaging**
- ⚡ WebRTC P2P — direct browser-to-browser when both online, zero relay
- 📡 Nostr async delivery — encrypted, decentralised, free, no account needed
- 📷 Steganography — dedicated screen to hide/reveal encrypted messages in PNG images
- 🔥 Auto-delete — messages expire after configurable time (1h / 24h / 7d)

**Privacy**
- 🔗 Key Transparency Log — local append-only Merkle chain, tamper-verifiable
- 🆔 No phone number, no email, no account — ever
- 📦 RAM-only mode — never writes to storage, wiped on page close

**Security Controls**
- Auto-wipe on tab close (optional)
- Auto-wipe on tab hide / phone lock (optional)
- Auto-wipe on 5 min inactivity (always on)
- DevTools detection wipe (default: on)

---

## Deploy in 5 Minutes

```bash
# 1. Create a GitHub repo named VOID
# 2. Upload index.html to the root
# 3. Settings → Pages → Deploy from branch: main / root
# 4. Your URL is live:
https://yourusername.github.io/VOID/
```

No server setup. No npm. No Docker. Nothing else needed.

---

## Using with Tor

1. Open Tor Browser
2. Navigate to `https://yourusername.github.io/VOID/`
3. Works immediately — GitHub Pages is reachable over Tor

For maximum anonymity, add a `.onion` Nostr relay to the `V.nostrRelays` array in `index.html`.

---

## Honest Security Statement

VOID is built on academically proven cryptographic protocols. However:

- ⚠️ Not independently audited by a third-party security firm
- ⚠️ JavaScript GC limits true cryptographic memory zeroing
- ⚠️ Traffic resistance ≠ true onion routing (Tor-grade)
- ⚠️ No software is "unhackable" — anyone who claims otherwise is lying

See `VOID_README.pdf` for the complete threat model.

---

## File Structure

```
VOID/
├── index.html       ← The entire application (single file, ~91KB)
└── VOID_README.pdf  ← Full technical + user guide
```

Two files. The whole thing.

---

## Technical Stack

| Layer | Technology |
|-------|-----------|
| Runtime | Browser WebCrypto API |
| Key Exchange | X3DH (ECDH P-256) |
| Messaging | Double Ratchet (HMAC-SHA256 chain) |
| Encryption | AES-256-GCM |
| P2P Transport | WebRTC DataChannel |
| Async Transport | Nostr (signed events, kind 30078) |
| Storage | IndexedDB (AES-256-GCM encrypted) |
| Passphrase | PBKDF2-SHA256 (600k iterations) |
| Nostr Signing | secp256k1 + BIP340 Schnorr (pure JS) |
| QR Codes | Pure JS (no library) |
| Steganography | LSB PNG encoding (Canvas API) |
| Dependencies | **Zero** |

---

<div align="center">

**VOID v3.0** · Single file · Zero dependencies · Zero servers you own

*Read every line of code. Verify everything. Trust nothing blindly.*

[![Open VOID](https://img.shields.io/badge/▶_Open_App-6C63FF?style=for-the-badge&labelColor=0A0A0F)](https://lanem187.github.io/VOID/)

</div>
