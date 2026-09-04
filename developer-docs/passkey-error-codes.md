# Passkey Error Codes

**Author:** Platform Security Engineering
**Date:** 2026-08-22
**Status:** Draft

---

Error codes returned by passkey-related endpoints. All errors are returned as JSON: `{ "error": "<CODE>", "message": "<human-readable description>" }`.

---

| Error code | HTTP status | Description | User action |
|---|---|---|---|
| `WEBAUTHN_CHALLENGE_EXPIRED` | 400 | The challenge TTL (5 min) was exceeded before the authentication or registration ceremony was completed | The user should try again from the beginning |
| `WEBAUTHN_ORIGIN_MISMATCH` | 400 | The credential origin does not match the expected relying party origin (`app.securepath.io`) | Should not occur in normal use; may indicate a misconfigured client or attempted attack |
| `WEBAUTHN_USER_VERIFICATION_FAILED` | 400 | The authenticator did not perform user verification (UV flag not set) | The user should re-attempt with biometric or PIN confirmation; may indicate device biometric not enrolled |
| `WEBAUTHN_DUPLICATE_CREDENTIAL` | 409 | A credential with the same credential ID already exists for this user | The user should use the existing passkey or remove it first |
| `WEBAUTHN_SIGNATURE_INVALID` | 401 | The assertion signature did not verify against the stored public key | May indicate a cloned credential or corrupted authenticator state; contact support |
| `WEBAUTHN_CREDENTIAL_NOT_FOUND` | 401 | The credential ID submitted in the assertion does not match any enrolled passkey for this user | The passkey may have been removed or the user is signing in on the wrong account |
| `WEBAUTHN_CREDENTIAL_REVOKED` | 401 | The credential ID matches a passkey that was revoked | The user should use a different passkey or sign in with TOTP |
| `WEBAUTHN_USER_VERIFICATION_REQUIRED` | 401 | The authenticator data indicates UV was not performed; UV is required by policy | The user should ensure their device biometric or PIN is enabled |
| `WEBAUTHN_SIGN_COUNT_ANOMALY` | — (logged only, not blocking in v4.2) | The sign count did not increment as expected | Logged for security review; may indicate a synced passkey (expected) or a cloned authenticator (investigate) |
| `PASSKEY_LIMIT_REACHED` | 422 | The user has reached the maximum number of enrolled passkeys | The user must remove an existing passkey before enrolling a new one |
| `PASSKEY_NOT_FOUND` | 404 | The specified credential ID does not exist in user-db | |

---

*FICTIONAL DOCUMENT — © JPDocu Services LTD. Created for educational purposes.*
