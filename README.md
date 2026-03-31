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

| Feature | Signal | WhatsApp | Telegram | ◌ VOID |
|---------|:------:|:--------:|:--------:|:------:|
| Phone number required | 😒 | 😒 | 😒  l
| 😄  |
| Central server | 😒  | 😒  | 😒  | 😄  |
| Metadata exposure | 😒 | 😒  | 😒 | 😄 |
| Relay for all messages | 😒 | 😒 | 😒 | 😄 |
| Panic wipe | 😒 | 😒  | 😒 | 😄 |
| Decoy account | 😒  | 😒 | 😒 | 😄 |
| Tor blocked | 😒 | 😒 | 😒 | 😄 |
| Closed source | 😒 | 😒 | 😒  | 😄 |
| Monthly cost | 😄  | 😄  | 😄  | 😄 Free |

> **VOID is the only messenger with all of these 😄. Signal comes closest on encryption — but Signal still requires your phone number, knows who you talk to, and when.**

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

         │         ┌─────────────────────────┐             │
         └────────►│   Nostr Relay Network   │─────────────┘
    (async/offline)│   Sees: ciphertext only │
                   │   Free · Decentralised  │
                   └─────────────────────────┘
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
- 🔐 Double Ratchet + X3DH — same protocol as Signal
- 🔑 Per-message forward secrecy — past messages safe even after compromise
- 🧅 Tor Browser compatible — works natively, no configuration needed
- ☠ Panic wipe — hold 3 seconds, everything gone instantly
- 🪬 Decoy account — dual passphrase, separate empty account under duress

**Messaging**
- ⚡ WebRTC P2P — direct browser-to-browser, zero relay when both online
- 📡 Nostr async delivery — encrypted, decentralised, free, no account
- 📷 Steganography — hide encrypted messages inside PNG images (LSB encoding)
- 🔥 Auto-delete — messages expire after configurable time (1h / 24h / 7d)

**Privacy**
- 📦 Fixed 4KB packets — message size hidden
- ⏱ Random 100–700ms delays — timing patterns hidden
- 👻 Dummy traffic — active/idle pattern hidden
- 🔗 Key Transparency Log — local append-only Merkle chain, tamper-verifiable

**Security Controls**
- Auto-wipe on tab close (optional)
- Auto-wipe on tab hide / phone lock (optional)
- Auto-wipe on 5 min inactivity
- DevTools detection wipe (default: on)
- RAM-only mode — never writes to IndexedDB

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
