# Passkeys Feature Overview — v4.2

**Author:** Platform Security Engineering
**Date:** 2026-08-15
**Status:** Draft — for TW review

---

## Background

SecurePath v4.2 introduces support for FIDO2 passkeys (also referred to as platform authenticators or WebAuthn resident keys in the spec). This replaces the optional TOTP-based 2FA second factor for users who want a more seamless authentication experience.

A passkey is a FIDO2 credential that is bound to a specific device (the authenticator) and a specific relying party (the SecurePath web application). The credential consists of a public/private key pair. The private key never leaves the device. The public key is registered with the SecurePath auth-service and stored in user-db.

When the user initiates authentication, the relying party sends a challenge. The device signs the challenge using the private key — which requires user verification (UV), typically a biometric gesture or device PIN. The auth-service verifies the signature against the stored public key.

Passkeys support two operating modes in v4.2:

1. **Second factor mode** — the user still enters a password first, then uses the passkey as the second factor instead of TOTP.
2. **Passwordless mode** — coming in v4.3, out of scope here.

---

## Why we built it

TOTP-based 2FA has known usability issues: users have to manually enter a 6-digit code within a 30-second window, which creates friction and support volume. FIDO2 passkeys eliminate the manual code entry. The user just taps their fingerprint or confirms with Face ID — the authentication happens in the background.

Additionally, WebAuthn passkeys are phishing-resistant by design. The credential is bound to the exact origin (`https://app.securepath.io`) — a phishing site on a different domain gets nothing, even if it looks identical. This is a significant security improvement over TOTP, which can be phished.

---

## What changed in the auth-service

The `auth-service` now supports a second authentication path:
- Existing: password → TOTP challenge → session JWT
- New: password → WebAuthn challenge → session JWT

The `enrollment-service` has new endpoints for WebAuthn registration ceremony (creating a new credential) and management (listing, deleting credentials per user).

A new component `webauthn-proxy` (Go, gRPC) wraps the [go-webauthn](https://github.com/go-webauthn/webauthn) library and handles the WebAuthn ceremony logic — challenge generation, credential verification, attestation parsing.

---

## Supported authenticator types

- Platform authenticators: Touch ID (macOS/iOS), Face ID (iOS), Windows Hello (Windows 10/11), Android biometrics (Android 9+)
- Roaming authenticators (cross-device): YubiKey 5 series, Google Titan Key (USB-A, USB-C, NFC)
- iCloud Keychain passkeys (iOS 16+, macOS Ventura+) — synced across Apple devices via iCloud

Note: SMS-based OTP remains unsupported. TOTP via authenticator app remains available for users who have not enrolled a passkey.

---

## Known constraints

- A user can enroll up to 5 passkeys per account (configurable by admin policy, default 5)
- Passkeys enrolled in second-factor mode cannot be promoted to passwordless (v4.3 feature)
- WebAuthn attestation is set to "none" for consumer accounts; "direct" for enterprise accounts with admin policy requiring verified authenticators

---

*FICTIONAL DOCUMENT — © JPDocu Services LTD. Created for educational purposes.*
