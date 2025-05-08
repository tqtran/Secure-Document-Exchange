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

---
## Attack Resistance Overview

This section outlines common attack scenarios and how the architecture defends against them. All evaluations assume standard threat models for secure file exchange, including compromised clients, servers, and communication channels.

| Attack Type                         | Resistance Level | Explanation                                                                                   |
|-------------------------------------|------------------|-----------------------------------------------------------------------------------------------|
| **Server compromise**               | ✅ High          | The server never possesses decryption keys or plaintext data. Only encrypted artifacts are stored. |
| **Inbox compromise**                | ⚠️ Partial       | Email contains SESSION_ID and image-based OTP. OTP must be manually extracted and entered, but full access is possible if the inbox is actively controlled. |
| **Replay or link reuse**            | ✅ Strong        | SESSION_ID and OTP are one-time use. Each expires quickly and cannot be reused once submitted. |
| **Phishing (fake portal)**          | ⚠️ Medium        | Image code verification, domain branding, and user prompts reduce phishing success, but a motivated attacker could recreate the flow. |
| **Brute-force SESSION_ID attempts** | ✅ Strong        | SESSION_IDs are derived from 256-bit entropy. IP-based rate limiting with weighted penalties is enforced. |
| **Session enumeration or timing attacks** | ✅ Strong    | No user-linked tokens, no predictable identifiers, and no pre-generated sessions to probe. |
| **Clipboard or session token malware** | ⚠️ Contextual  | Tokens require interactive OTP input tied to image visibility. Passive harvesting is not sufficient without full session context. |
| **Tampering with uploaded content** | ✅ Strong        | Files are encrypted client-side before upload. Recipients validate authenticity by controlling the key and chain of trust. |

This architecture focuses on protecting the content even under partial compromise of infrastructure or delivery channels, while maintaining usability and zero-knowledge guarantees.
