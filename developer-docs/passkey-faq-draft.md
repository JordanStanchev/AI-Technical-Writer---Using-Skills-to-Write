# Passkey FAQ — Draft

**Author:** Support Engineering
**Date:** 2026-08-24
**Status:** Draft — pulled from support ticket analysis, needs TW rewrite

---

These are the questions our support team is already getting in beta. Written up quickly from ticket notes — needs proper formatting and style guide review before publishing.

---

**Q: What is a passkey?**
A: A passkey is a more secure and easier-to-use alternative to TOTP (authenticator app) codes. Instead of typing a 6-digit code, the user uses their device's built-in biometric sensor (fingerprint, face recognition) or PIN to authenticate. The passkey is a FIDO2 credential stored securely on the device.

**Q: Do I need to install anything to use passkeys?**
A: No. Passkeys use the biometric sensors and security chips already built into modern devices. On macOS, it uses Touch ID. On iPhone/iPad, it uses Face ID or Touch ID. On Windows, it uses Windows Hello. No additional app is required.

**Q: Can I use passkeys on multiple devices?**
A: Yes. The user can enroll a passkey on each device they want to use for signing in, up to the maximum allowed by their plan (default: 5 passkeys per account). Alternatively, if the user uses iCloud Keychain (Apple devices) or Google Password Manager (Android), passkeys may sync automatically across their devices.

**Q: What happens if I lose my phone or device?**
A: The user should immediately sign in from another device. If they have another passkey enrolled on a different device (recommended), they can use that. If not, they can fall back to their authenticator app TOTP code or a backup code. If none of these are available, account recovery via email is possible — see the account recovery documentation.

**Q: Is passkey more secure than my authenticator app?**
A: For most users, yes. Passkeys are resistant to phishing — the credential is cryptographically bound to the SecurePath domain, so even if a user is tricked into visiting a fake login page, the passkey will not work on that site. TOTP codes, by contrast, can be entered on a phishing site.

**Q: Can my admin force me to use passkeys?**
A: Yes, if the user's organisation has enabled the "Require passkey" policy. The user will be notified and given a grace period to enroll before sign-in is restricted.

**Q: Can I use a YubiKey as a passkey?**
A: Yes. YubiKey 5 series and Google Titan Keys are supported as external FIDO2 authenticators. The user plugs in the key, taps it when prompted, and the key generates the authentication response. This is a good option for users who want a hardware-bound credential that does not sync to any cloud.

**Q: What is the difference between a passkey and two-factor authentication?**
A: In SecurePath v4.2, passkeys are used as the second factor — the user still enters their password first. The passkey replaces the TOTP code step. Future versions will support passkeys as the only required authentication step (passwordless sign-in), eliminating the password entirely.

**Q: I enrolled a passkey but I'm still being asked for my TOTP code.**
A: This usually happens when the user is signing in from a browser or device that does not have access to the enrolled passkey. The user should check that they are signing in on the same device where the passkey was enrolled, or that their passkey is synced to the current device via iCloud Keychain or Google Password Manager.

**Q: Can I remove a passkey?**
A: Yes. Under Settings > Security > Manage Passkeys, the user can remove any enrolled passkey. Removing all passkeys does not disable 2FA — TOTP will become the active second factor again.

---

*FICTIONAL DOCUMENT — © JPDocu Services LTD. Created for educational purposes.*
