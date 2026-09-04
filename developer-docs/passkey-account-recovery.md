# Account Recovery — Passkey Device Loss

**Author:** Platform Security Engineering
**Date:** 2026-08-21
**Status:** Draft

---

## Scenario

The user has a passkey enrolled and either:
(a) has lost the device the passkey was stored on, or
(b) has reset their device (factory reset, reinstall) and the passkey was not synced to a cloud keychain

In both cases the passkey private key is no longer accessible and the credential cannot be used for authentication.

## Recovery options (in order of preference)

### Option 1 — Use another enrolled passkey

If the user has more than one passkey enrolled (e.g., a YubiKey as backup), they can use the other passkey to sign in. On the passkey prompt, if no local passkey matches, most browsers will offer "Use a passkey from a different device" (cross-device authentication via caBLE/Hybrid transport). The user can select their other device.

### Option 2 — Use TOTP

If the user has an authenticator app enrolled alongside the passkey, clicking "Use a different method" at the passkey challenge will offer TOTP. The user enters the 6-digit code from their authenticator app.

### Option 3 — Use a backup code

From the "Use a different method" screen, the user can select "Use a backup code". The 8-character backup code is entered. The backup code is invalidated on use.

### Option 4 — Account recovery via email

If none of the above are available (no backup passkey, no TOTP, no backup codes), the user must go through account recovery:

1. On the sign-in screen, after entering username and password, the user clicks "Lost access to all methods?"
2. The user is prompted to confirm their email address.
3. A recovery link is sent to the verified email address on file. TTL: 15 minutes.
4. The user follows the link and must answer identity verification questions (or, for enterprise accounts, an admin can trigger the recovery on the user's behalf).
5. On successful identity verification, all 2FA methods are temporarily suspended and the user is signed in with a degraded session (read-only, limited to security settings page).
6. The user is required to enroll a new 2FA method before full access is restored.

Note: during recovery, the user's existing passkeys remain enrolled but are flagged as suspicious. A security notification is sent to the account email.

## Admin recovery path

An admin can suspend 2FA for a specific user from Admin Console > Users > [user] > Security > Suspend 2FA. This grants the user a 24-hour window to sign in without 2FA and re-enroll. The action is logged in the audit trail.

---

*FICTIONAL DOCUMENT — © JPDocu Services LTD. Created for educational purposes.*
