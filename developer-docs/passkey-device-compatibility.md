# Passkey Device Compatibility

**Author:** Platform Security Engineering
**Date:** 2026-08-15
**Status:** Draft — needs TW review for formatting

---

The following platforms and authenticator types have been tested with the SecurePath passkey implementation. The list reflects test coverage at time of v4.2 release. Untested configurations may work but are not officially supported.

## Platform authenticators (built-in)

These use the device's secure enclave and biometric sensors. The private key is stored on the device and does not sync unless the platform provides a sync mechanism (e.g., iCloud Keychain).

| Platform | Authenticator | Min version | Notes |
|---|---|---|---|
| macOS | Touch ID | macOS 13 (Ventura) | Also works with Magic Keyboard with Touch ID |
| iOS / iPadOS | Face ID / Touch ID | iOS 16 | Syncs via iCloud Keychain if enabled |
| Windows | Windows Hello | Windows 10 21H2 | PIN, fingerprint, facial recognition all supported |
| Android | Fingerprint / face unlock | Android 9 | Passkeys sync via Google Password Manager on Android 14+ |
| ChromeOS | Fingerprint | ChromeOS 120 | Limited to Chromebook devices with fingerprint sensor |

## Roaming authenticators (external hardware keys)

These are physical USB or NFC keys. The private key is stored on the hardware key itself, never on the host device.

| Authenticator | Interface | Notes |
|---|---|---|
| YubiKey 5 NFC | USB-A, NFC | FIDO2 + TOTP on same key; most commonly used in enterprise |
| YubiKey 5C NFC | USB-C, NFC | Same as above, USB-C form factor |
| YubiKey 5Ci | USB-C + Lightning | For iOS users with YubiKey preference |
| Google Titan Key | USB-A, USB-C, NFC | Available separately; NFC version recommended |

## Browsers

All major browsers that implement the WebAuthn Level 2 API are supported. Minimum tested versions:

| Browser | Min version |
|---|---|
| Chrome / Chromium | 108 |
| Safari | 16.1 |
| Firefox | 119 |
| Edge | 108 |
| Arc | latest (Chromium-based) |

Safari on iOS requires iOS 16+ for passkey support regardless of browser used (all iOS browsers use WebKit).

## Known incompatibilities

- Internet Explorer: not supported (no WebAuthn implementation)
- Firefox on iOS: uses WebKit like all iOS browsers; passkey support depends on iOS version
- Samsung Internet < 20: untested; WebAuthn support inconsistent in older versions

---

*FICTIONAL DOCUMENT — © JPDocu Services LTD. Created for educational purposes.*
