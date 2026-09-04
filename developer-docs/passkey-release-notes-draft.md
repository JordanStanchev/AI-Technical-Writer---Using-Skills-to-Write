# Release Notes Draft — SecurePath v4.2 (Passkeys)

**Author:** Product Management
**Date:** 2026-08-25
**Status:** Draft — TW to review and rewrite for external audience

---

## What's new in v4.2

### Passkey support (FIDO2/WebAuthn)

SecurePath v4.2 ships support for FIDO2 passkeys as a second authentication factor. Users who previously used TOTP (Google Authenticator, Authy, etc.) can now add a passkey and authenticate using their device's biometric sensor or PIN — no more manually typing 6-digit codes.

Passkeys are more secure than TOTP against phishing attacks, and faster for day-to-day use. In enterprise environments, admins can enforce passkey enrollment org-wide via the new policy controls in Admin Console.

This is the first step toward full passwordless authentication, which is planned for v4.3.

### What's included in v4.2

- Passkey enrollment from Settings > Security > Two-Factor Authentication > Add Passkey
- Passkey authentication as second factor (password + passkey)
- Support for platform authenticators (Touch ID, Face ID, Windows Hello, Android biometrics)
- Support for roaming authenticators (YubiKey 5 series, Google Titan Key)
- iCloud Keychain passkey sync support (iOS 16+, macOS Ventura+)
- Up to 5 passkeys per account
- Friendly name for each passkey
- Passkey management: view, rename, remove
- Fallback to TOTP or backup codes if passkey device is unavailable
- Admin policy: require passkey, restrict to verified authenticators, configure max passkeys
- Account recovery flow for users who lose all authentication methods

### Not in this release

- Passwordless sign-in (passkey without password) — planned v4.3
- Admin delegation for passkey recovery — planned v4.3
- Passkey analytics in Admin Console — planned v4.3

---

## Bug fixes and improvements

- Fixed an issue where the TOTP enrollment UI showed an error if the user's clock was more than 60 seconds out of sync (PAK-adjacent fix)
- Improved error messages on the 2FA settings page — specific error codes are now shown instead of generic "An error occurred"
- Admin Console: user security overview now shows passkey enrollment count alongside TOTP status

---

*FICTIONAL DOCUMENT — © JPDocu Services LTD. Created for educational purposes.*
