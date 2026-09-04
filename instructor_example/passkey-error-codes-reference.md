# Passkey Error Codes Reference

## Summary

This reference lists the error codes you may encounter when setting up or using a passkey in SecurePath, with a description of what each means and what to do next.

## Error codes

| Error code | What it means | What to do |
|---|---|---|
| `WEBAUTHN_CHALLENGE_EXPIRED` | The passkey setup or sign-in took longer than 5 minutes to complete. | Start the process again from the beginning. |
| `WEBAUTHN_USER_VERIFICATION_FAILED` | Your device did not confirm your identity. This can happen if you do not complete the biometric or PIN prompt. | Try again and make sure you complete the fingerprint, face, or PIN confirmation when prompted. |
| `WEBAUTHN_DUPLICATE_CREDENTIAL` | You are trying to set up a passkey that is already enrolled on this account. | You do not need to set it up again. Go to **Settings** > **Security** to see your existing passkeys. |
| `WEBAUTHN_SIGNATURE_INVALID` | SecurePath could not verify the passkey response from your device. | Try signing in again. If the issue continues, remove the passkey and set it up again. |
| `WEBAUTHN_CREDENTIAL_NOT_FOUND` | The passkey on your device does not match any passkey registered on this account. | Make sure you are signing in to the correct account. If you recently removed a passkey, set it up again. |
| `WEBAUTHN_CREDENTIAL_REVOKED` | This passkey has been removed from your account. | Set up a new passkey or sign in using your authenticator app or a backup code. |
| `PASSKEY_LIMIT_REACHED` | You have reached the maximum number of passkeys allowed on your account. | Remove an existing passkey from **Settings** > **Security** before adding a new one. |
| `PASSKEY_NOT_FOUND` | SecurePath cannot find the passkey you are trying to update or remove. | Refresh the page and try again. |

---

*© JPDocu Services LTD | JPDocu School of Technical Writing | www.jpdocu.com*
