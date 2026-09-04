# Admin Policy — Passkey Enforcement

**Author:** Platform Security Engineering
**Date:** 2026-08-22
**Status:** Draft — needs PM review on policy naming

---

## Overview

Enterprise admins can configure passkey enforcement policies at the organisation level via the Admin Console. Policies take effect for all members of the org. Individual user settings can be overridden by org policy.

---

## Available policies

### Require passkey for all members

When enabled, all org members must enroll at least one passkey before their next sign-in after the policy is set. If a member does not have a passkey, they are redirected to the enrollment flow after password authentication and cannot proceed until a passkey is enrolled.

Location: Admin Console > Settings > Security > Authentication > Require passkey: ON

Grace period: the admin can set a grace period (1–30 days) during which the member sees a reminder but is not blocked. After the grace period, sign-in is blocked until a passkey is enrolled.

### Restrict to verified authenticators only

When enabled, only hardware security keys with verified attestation are accepted during enrollment. Platform authenticators (Touch ID, Windows Hello) that return "none" attestation are rejected.

This policy is intended for high-security environments that require hardware-bound credentials. It prevents the use of synced passkeys (e.g., iCloud Keychain passkeys).

Location: Admin Console > Settings > Security > Authentication > Require verified authenticator: ON

Note: this policy can be highly disruptive to users who primarily use platform authenticators. It is recommended only for orgs with a hardware key distribution policy already in place.

### Maximum passkeys per user

Admins can set the maximum number of passkeys a user can enroll. Default is 5. Minimum is 1. This applies org-wide and overrides the platform default.

Location: Admin Console > Settings > Security > Authentication > Max passkeys per user

### Allow TOTP fallback

By default, users with passkeys enrolled can still fall back to TOTP if their passkey is unavailable. Admins can disable this fallback for the org, meaning passkey is the only accepted second factor. If the user cannot use their passkey, they must go through account recovery.

Disabling TOTP fallback increases security but also increases support load. Recommended only for orgs where users are expected to have at least two passkeys enrolled (primary + backup hardware key).

Location: Admin Console > Settings > Security > Authentication > Allow TOTP fallback: OFF

---

## Audit logging

All admin policy changes are logged to the org audit log with timestamp, admin user ID, policy name, old value, and new value.

---

*FICTIONAL DOCUMENT — © JPDocu Services LTD. Created for educational purposes.*
