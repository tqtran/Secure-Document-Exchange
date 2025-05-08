## Frequently Asked Questions (FAQ)

### Why isn’t everything just encrypted on the server?
Because we don’t want the server to be able to decrypt anything on its own. Decryption requires secrets only the user possesses.

### What happens if someone forwards the email?
The recipient would still need to type the OTP/image code. But if they forward the whole bundle and it’s used in real time, it can be exploited.

### Can this system be phished?
Yes — but image-based session validation and OTP input increase the difficulty. Domain verification, code uniqueness, and short TTLs help reduce the window.

### What if the user never opens the email?
Session creation only begins when the user opens the email and the image loads. No session = no decryption path = no attack surface.

### Why not use client-side decryption?
Possible, but intentionally avoided due to encoding inconsistencies, platform compatibility, and user complexity. Server-based decryption with zero-knowledge enforcement offers stronger UX/security balance.

### Is this system compliant with privacy laws?
Yes. It avoids long-term storage, PII, and tracking. Design adheres to data minimization and short-lived encrypted sessions aligned with GDPR/CCPA.

