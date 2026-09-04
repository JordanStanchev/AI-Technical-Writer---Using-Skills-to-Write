# What Is a Passkey

## Summary

A passkey is a secure sign-in method that lets you authenticate to SecurePath using your device's fingerprint sensor, face recognition, or PIN — instead of a time-based code from an authenticator app.

## Overview

When you sign in to SecurePath with a passkey, your device does the work. There is no code to type and no app to open. You confirm your identity with the same gesture you use to unlock your phone or laptop — a fingerprint, a glance at your camera, or your device PIN — and you're in.

Under the surface, a passkey is a cryptographic key pair. When you set up a passkey, your device creates two mathematically linked keys:

- The **private key** is stored securely on your device and never leaves it.
- The **public key** is sent to SecurePath and stored on our servers.

When you sign in, SecurePath sends your device a unique challenge. Your device signs it with the private key — which requires your biometric or PIN to unlock — and sends the signed response back. SecurePath verifies the signature using the public key. If it matches, you're authenticated.

Because the private key never leaves your device, there is nothing for an attacker to steal from a server breach. And because the passkey is tied to the exact SecurePath website address, it will not work on a phishing site that mimics the login page — even if the fake site looks identical.

<!-- IMAGE: diagram showing the passkey sign-in flow: device ↔ SecurePath, with private key on device and public key on server -->

## Example

You open the SecurePath sign-in page on your MacBook. You enter your email and password. SecurePath prompts you to confirm with your passkey. Your MacBook displays the Touch ID prompt. You rest your finger on the sensor. SecurePath verifies the response and signs you in — no code, no waiting, no second device required.

---

*© JPDocu Services LTD | JPDocu School of Technical Writing | www.jpdocu.com*
