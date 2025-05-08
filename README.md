# Secure Document Exchange

This project implements a secure, zero-knowledge document exchange protocol that ensures files can only be accessed by intended recipients — not by the server or attackers with partial access. It is designed to be implementation-agnostic, privacy-preserving, and cryptographically robust.

## 🔐 Key Features

- AES-256 per-file encryption
- Session-based decryption using split-key delivery
- Image-based session initialization and access token
- OTP validation to prevent link forwarding and reuse
- Fully ephemeral: auto-purges after 2 calendar days
- No server-held logs or long-term key material
- Resistant to phishing, brute-force, and replay attacks

## 📦 How It Works (Quick Summary)

1. The sender uploads a file, which is encrypted using a unique key.
2. The server encrypts the decryption key and package reference using a temporary server key.
3. That temporary key is then encrypted using a session decryption key, and only the client holds the full path to recovery.
4. When the recipient opens the email, an image is loaded that generates a session token on the server.
5. The recipient then visits the portal, enters the image-based code, and retrieves the encrypted package.
6. The server decrypts the key layers only when all components are provided.

## 🧾 Documentation

For a full technical breakdown, see [docs/architecture.md](docs/architecture.md).

Includes:
- Non-technical analogy
- Encryption flow and data lifecycle
- Security properties and residual risks
- Frequently asked questions

## ✅ Status

This is a concept-stage architecture, ready for implementation in any stack (Node, Python, Go, etc.). Contributions welcome.

## 📄 License

[MIT](LICENSE)
