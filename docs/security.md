## Security and Risk Analysis

### Achieved Security Properties

| Property                    | Status     | Description                                                                                                          |
| --------------------------- | ---------- | -------------------------------------------------------------------------------------------------------------------- |
| Zero-knowledge server       | ✅          | Server cannot decrypt without user-supplied keys.                                                                   |
| Key uniqueness              | ✅          | Each file is encrypted with a unique AES-256 key.                                                                   |
| Brute-force resistance      | ✅          | SESSION_DECRYPTION_KEY is 256-bit; server applies rate-limiting with penalty weights.                              |
| Replay protection           | ✅          | OTP/image-based session and SESSION_IDs are single-use and expire.                                                  |
| Email compromise mitigation | ⚠️ Partial | Requires OTP/code input, but if inbox is fully compromised, all secrets are exposed.                               |
| Logging/privacy             | ✅          | No access logs; no session metadata stored beyond expiration.                                                       |
| Post-expiry destruction     | ✅          | Full purge cycle after 2 calendar days.                                                                             |

### Residual Risks

| Risk                       | Likelihood  | Mitigation                                                                                                                   |
| -------------------------- | ----------- | ---------------------------------------------------------------------------------------------------------------------------- |
| Full inbox compromise      | Medium–High | Image and SESSION_ID both come from email. Consider split-channel delivery in sensitive deployments.                         |
| Phishing (fake portal)     | Medium      | Image token must match server session. Encourage users to verify URL and image presence.                                     |
| Link sharing / token reuse | Low         | SESSION_IDs and OTPs expire; single-use enforcement is in place.                                                             |
| DOS/brute-force attempts   | Low         | Rate limits per IP + session token with decay scoring block automated abuse.                                                 |
