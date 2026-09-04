# Passkey Security Considerations

**Author:** Platform Security Engineering
**Date:** 2026-08-23
**Status:** Draft — for TW and PM review

---

## Threat model summary

This document summarises the security properties of the passkey implementation in SecurePath v4.2 for documentation and communication purposes. It is not a full security audit — refer to the security architecture document for the complete threat model.

---

## What passkeys protect against

### Phishing

This is the primary security advantage of passkeys over TOTP. A WebAuthn credential is cryptographically bound to the exact relying party origin (`app.securepath.io`). If a user visits a phishing site (e.g., `app-securepath.io`) and is prompted for a passkey, the browser will not find a matching credential and authentication will fail. The attacker gets nothing.

TOTP codes, by contrast, can be phished in real time — an attacker can forward the TOTP code to the real site within the 30-second window.

### Credential stuffing

Passkeys do not involve a shared secret transmitted over the network. The private key never leaves the device. There is nothing to intercept, leak, or reuse on another site.

### Replay attacks

Each authentication challenge is unique and single-use. A captured assertion response cannot be replayed — the challenge is invalidated after first use.

---

## What passkeys do NOT protect against

### Compromised device

If an attacker gains physical access to an unlocked device (or can bypass the biometric/PIN), they can authenticate using the passkey. Device-level security (strong PIN, automatic lock, full disk encryption) remains important.

### Account takeover after device loss (if no fallback is configured)

If a user has only one passkey enrolled and no backup method (TOTP, backup codes), losing the device could result in account lockout. This is addressed by the account recovery flow, but the recovery process introduces its own attack surface (compromised email account).

**Recommendation for documentation:** advise users to enroll at least two passkeys (e.g., device passkey + hardware key) or to keep backup codes stored securely.

### Synced passkeys (iCloud Keychain, Google Password Manager)

Synced passkeys provide better recovery (passkey is accessible on all the user's Apple/Google devices) but reduce the hardware-binding guarantee. If iCloud or Google account is compromised, the attacker could potentially access the synced passkeys.

For users in high-security environments, hardware-key-based passkeys (YubiKey) are preferable to synced passkeys.

---

## Key security parameters (v4.2)

| Parameter | Value |
|---|---|
| User verification | Required (UV flag must be set) |
| Attestation (consumer) | None |
| Attestation (enterprise with policy) | Direct |
| Challenge length | 32 bytes (256 bits) |
| Challenge TTL | 5 minutes |
| Sign count anomaly handling | Log only (not blocking) in v4.2 |
| Maximum passkeys per user | 5 (default, configurable by admin) |

---

*FICTIONAL DOCUMENT — © JPDocu Services LTD. Created for educational purposes.*
