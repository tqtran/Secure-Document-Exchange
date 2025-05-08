# Secure Document Exchange Architecture

## How It Works (For Non-Technical Readers)

Imagine a bank where you want to store a confidential document in a deposit box for someone else to retrieve — but you don’t want anyone, including the bank, to know the contents or be able to tamper with them. Here's how the system works using that analogy:

1. The bank locks your document into a secure storage bin.
2. The bank then puts the storage bin ID and key into a separate lockbox.
3. The key to that lockbox is placed into yet another container — a key box.
4. You send the recipient the key to the key box, its identifier, and the lockbox itself.
5. When the recipient visits the bank, they present the key and ID for the key box.**
6. The bank then uses that key to open the lockbox and retrieve the original storage bin ID and its key.
7. That key is then used to unlock the storage bin and retrieve the file.
8. All keys and boxes are destroyed after two days, used or not.

This ensures the bank cannot tamper with your contents, doesn’t know which bin is yours, and can’t access or modify the file without the exact chain of keys provided by the sender.

---

## Technical Architecture Overview

### 1. Encryption and Storage

- The file is encrypted with a one-time `PACKAGE_KEY`.
- `{PACKAGE_GUID, PACKAGE_KEY}` is encrypted into a `BUNDLE` using `TEMP_SERVER_KEY`.
- `TEMP_SERVER_KEY` is encrypted with a user-held `SESSION_DECRYPTION_KEY`, creating the `ENCRYPTED_SERVER_KEY`.

### 2. Delivery to the Recipient

- `SESSION_ID` is created from `SERVER_KEY_ID` + `SESSION_DECRYPTION_KEY` (user-held).
- An image-based code is generated when the recipient opens the email, and this action triggers session creation.
- The email contains the image and access link to the retrieval portal.

### 3. Retrieval and Validation

- The recipient enters the visible code from the email.
- Server uses that to find and decrypt the session materials:
  - Decrypts `ENCRYPTED_SERVER_KEY` with the user-supplied key
  - Uses result to decrypt `BUNDLE`, revealing `PACKAGE_KEY`
  - Uses `PACKAGE_KEY` to decrypt the original file

### 4. Expiry and Cleanup

- All stored content (files, keys, bundles) is purged after two calendar days.
