# Passkey Known Issues — v4.2

**Author:** Platform Security Engineering
**Date:** 2026-08-25
**Status:** Draft — keep this internal, check with PM before publishing

---

## PAK-001 — iCloud Keychain passkeys: sign count anomaly on cross-device use

**Severity:** Low (logged, not blocking)

When a passkey is synced via iCloud Keychain and used on a different Apple device than the one that enrolled it, the WebAuthn sign count may not increment sequentially. This triggers `WEBAUTHN_SIGN_COUNT_ANOMALY` in the server logs. Authentication is not blocked in v4.2.

This is expected behaviour for synced passkeys. The WebAuthn spec notes that synced credentials may not maintain a reliable sign counter. The security impact is low because iCloud Keychain sync itself is protected by Apple's end-to-end encryption.

**Workaround:** None needed. The log entry is informational.

**Fix plan:** v4.3 — sign count anomaly will not be logged for credentials enrolled via platform authenticators with known sync behaviour (identified via attestation metadata).

---

## PAK-002 — Firefox on Windows: UV prompt timing out on slow hardware

**Severity:** Medium

On Windows machines with slow fingerprint sensors (some older laptops), Windows Hello sometimes takes longer than the WebAuthn timeout (60 seconds, which should be sufficient). In rare cases the browser returns a timeout error before the gesture is accepted.

**Workaround:** The user can retry. If the issue is persistent, the user can use a PIN instead of fingerprint as the UV gesture — PIN verification is faster.

**Fix plan:** Investigating whether the timeout can be extended for specific authenticator types. No ETA.

---

## PAK-003 — Samsung Internet browser: passkeys not supported

**Severity:** Medium

Samsung Internet (default browser on older Samsung Android devices) does not reliably support the WebAuthn API. Passkey enrollment and authentication will fail with a browser-level error that does not map to a SecurePath error code.

**Workaround:** The user should use Chrome or another Chromium-based browser on the affected device.

**Fix plan:** No fix planned on the SecurePath side. Samsung Internet WebAuthn support depends on Samsung's browser team.

---

## PAK-004 — Friendly name not persisted on initial enrollment if left blank

**Severity:** Low

If the user completes passkey enrollment without entering a friendly name, the credential is stored with `friendlyName: null`. The UI correctly displays "Unnamed passkey" in this case. However, if the user subsequently tries to update the friendly name via `PATCH /api/v1/passkeys/{credentialId}` and the credential was enrolled with `null`, a 500 error is returned in v4.2.0. Fixed in v4.2.1 (hotfix applied to production on 2026-08-27).

**Workaround (before v4.2.1):** Enroll with a friendly name set. After v4.2.1 hotfix, patching credentials with null friendly names works correctly.

---

*FICTIONAL DOCUMENT — © JPDocu Services LTD. Created for educational purposes.*
