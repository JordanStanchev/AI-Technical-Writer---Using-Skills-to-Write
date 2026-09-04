# Passkey and Backup Codes — Interaction Notes

**Author:** Platform Security Engineering
**Date:** 2026-08-20
**Status:** Draft — internal only, needs significant TW rewrite

---

This doc covers how the passkey feature interacts with the existing backup code system. Some of this is non-obvious so writing it down before the TW team asks.

## Background

When a user enrolls TOTP (authenticator app), 10 backup codes are generated and shown once. These backup codes can be used as a fallback if the user loses their authenticator app. Backup codes are HOTP-based (counter-incremented), stored as bcrypt hashes.

When a user enrolls a passkey, backup codes are NOT regenerated. The user keeps whatever backup codes they have from their TOTP enrollment. Backup codes work independently of whether the user has a passkey enrolled or not.

## Why this matters

If a user has both TOTP and a passkey enrolled, during sign-in they will be prompted for the passkey (passkey takes priority over TOTP challenge when enrolled). But if the passkey device is unavailable, the user can click "Use a different method" and fall back to TOTP, and then use a backup code if needed.

So the fallback chain is: passkey → TOTP → backup code.

## Backup code state after passkey-only enrollment

Edge case: what if a user skips TOTP entirely and goes straight to passkey enrollment? This is possible in v4.2 because passkey enrollment does not require prior TOTP enrollment.

In this case, the user has no backup codes. If they lose their passkey device, their only recovery path is the account recovery flow (see passkey-account-recovery.md).

We should probably mention in the UI that it is recommended to set up backup codes even if using passkeys. This is a product recommendation, not a tech constraint — flagging for PM review.

## Regenerating backup codes

Backup codes can be regenerated at any time from Settings > Security > Backup Codes. Regenerating invalidates all previous codes. This can be done regardless of whether passkeys are enrolled.

## Code-level interaction

The backup code validation path is entirely separate from the WebAuthn path in `auth-service`. There is no code dependency between them. The only interaction is at the UX level — the "Use a different method" option the frontend shows when passkey challenge is active.

---

*FICTIONAL DOCUMENT — © JPDocu Services LTD. Created for educational purposes.*
