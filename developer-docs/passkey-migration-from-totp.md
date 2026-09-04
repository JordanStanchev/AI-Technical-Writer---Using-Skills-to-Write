# Migration Guide — From TOTP to Passkeys

**Author:** Platform Security Engineering
**Date:** 2026-08-23
**Status:** Draft — this one is important, lots of users will be affected

---

## Who this is for

This is for users who are currently using TOTP-based 2FA (authenticator app) and want to switch to passkeys. It is also relevant for admins who are planning an org-wide migration.

Note: migrating to passkeys does not remove TOTP. Both methods can remain active simultaneously. The user can remove TOTP afterward if they prefer a passkey-only setup.

---

## What changes for the user

Currently, after entering username and password, the user is prompted to enter a 6-digit TOTP code. After enrolling a passkey, the user will be prompted for the passkey instead (passkey takes precedence over TOTP when both are enrolled). The user can still fall back to TOTP by clicking "Use a different method" if the passkey device is unavailable.

---

## Steps to add a passkey

1. The user should sign in to SecurePath using their existing credentials (username + password + TOTP code).
2. Navigate to Settings > Security > Two-Factor Authentication.
3. Click Add Passkey.
4. The browser will request a biometric gesture or device PIN to create the credential.
5. Optionally, enter a friendly name for the passkey (e.g., "Personal MacBook", "YubiKey").
6. Click Save.

The passkey is now enrolled. The next sign-in will prompt for the passkey instead of TOTP.

---

## Steps to remove TOTP after passkey enrollment

TOTP removal is only recommended once the user has confirmed the passkey is working correctly and has enrolled backup codes (or an additional passkey) as a fallback.

1. Navigate to Settings > Security > Two-Factor Authentication.
2. Locate the Authenticator App section.
3. Click Remove.
4. A confirmation dialog is shown warning that TOTP will no longer be available as a fallback.
5. Confirm removal.

Note: removing TOTP does not remove backup codes. Backup codes remain valid until used or regenerated.

---

## Admin-managed migration

For orgs migrating all users to passkeys, the recommended approach is:

1. Communicate to users that passkeys will be required by a specific date.
2. Enable the "Require passkey" policy with a grace period matching the communication timeline.
3. During the grace period, users will see a banner prompting them to enroll a passkey but will not be blocked.
4. After the grace period, users without a passkey will be required to enroll one at their next sign-in.

It is strongly recommended to keep TOTP fallback enabled during and for at least 30 days after the migration to reduce support volume from users who lose access to their passkey device during the transition.

---

*FICTIONAL DOCUMENT — © JPDocu Services LTD. Created for educational purposes.*
