# Removing a Passkey

**Author:** Platform Security Engineering
**Date:** 2026-08-19
**Status:** Draft

---

A passkey can be removed from the user's account from the Security settings page. Multiple passkeys can be removed independently. Removing all passkeys does not disable 2FA — the user's TOTP enrollment remains active if it was configured.

## How removal works

The user should go to Settings > Security > Manage Passkeys. A list of enrolled platform authenticators is displayed, showing the friendly name (if set), device type (if detectable), and enrollment date.

The user selects the passkey to remove and clicks the Remove button. A confirmation dialog is shown. The user confirms removal.

`DELETE /api/v1/passkeys/{credentialId}` is called. The `enrollment-service` sets the credential record status to `REVOKED` in `user-db`. The credential is not deleted from the database — it is flagged as revoked so that any future authentication attempt using the revoked credential ID is rejected with `WEBAUTHN_CREDENTIAL_REVOKED`.

The removal does not invalidate existing active sessions. If the user wants to end all sessions (e.g., because the device was lost), they should use the separate "Sign out all devices" function under Settings > Security.

## Admin-initiated removal

Admins can remove passkeys on behalf of users via the Admin Console at Settings > Users > [user] > Security > Manage Passkeys. The same revocation logic applies. An audit log entry is created under the admin's account and the user's account.

---

*FICTIONAL DOCUMENT — © JPDocu Services LTD. Created for educational purposes.*
