# SolGuard

**Verify the recipient before you send.**

SolGuard is a Chrome extension MVP for inspecting Solana payment data before a user sends funds. It compares the recipient displayed on a checkout page with the destination decoded from its QR code, checks available payment amounts, and flags a SOL-versus-SPL-token inconsistency when detectable.

## Current version

Use **[solguard-update-qrcode.zip](./solguard-update-qrcode.zip)**, which contains the QR-enabled extension **v1.5.1**.

- `solguard-update-qrcode.zip`: current QR-enabled extension.
- `SolGuard-Chrome-Extension (7).zip`: earlier extension, v1.3.0.
- The source files are currently packaged inside these ZIP archives.

## Install in Chrome

1. Download `solguard-update-qrcode.zip` from this repository and extract it.
2. Open `chrome://extensions`.
3. Enable **Developer mode**.
4. Click **Load unpacked**.
5. Select the extracted **`solguard-update-qrcode`** folder containing `manifest.json`.
6. Pin SolGuard using Chrome's Extensions menu.
7. Reload any checkout page that was already open before installation.

The bundled extension requires Chrome 102 or later. No npm install, build step, custom backend, or wallet connection is required to inspect payment data.

## Use and review the MVP

### Automatic checkout analysis

1. Open a supported Solana checkout that displays a recipient address and a QR code.
2. Keep the checkout tab active and its QR code visible.
3. SolGuard scans the page and attempts to decode the QR.
4. Open the extension popup to inspect the recipient, amount, token, available checks, and warning details.

For a controlled demonstration, compare a matching checkout/QR pair, then change only the QR recipient or amount. A readable mismatch should produce a red warning. Missing or unreadable QR data can produce an inconclusive result.

### Manual link inspection

1. Open the SolGuard popup.
2. Paste a valid Solana Pay link or Solana address into **“Link Solana Pay ou endereço”**.
3. Click **“Analisar pagamento”**.
4. Inspect the parsed recipient, amount, and token, and compare them with the merchant's information.

Manual input does not independently authenticate a merchant or compare a QR with a checkout. It checks the input format and the local reference list. No real payment is needed to inspect these results.

## Result colors

| Color | Meaning |
| --- | --- |
| Blue | The QR recipient matches the displayed recipient, with no mismatch detected in the fields this version compares. |
| Green | The recipient matches an entry in the static `DEFAULT_REGISTRY` reference list. |
| Yellow | Verification is inconclusive, data is missing or unreadable, or the recipient is not independently identified. |
| Red | A mismatch is detected in the recipient, comparable amount, or supported token check. |

**Matching data or a known reference does not guarantee merchant safety.** The bundled registry contains a demonstration entry, not a merchant identity certification service.

## Processing and permissions

QR decoding and payment comparisons run locally in the browser. Analysis results are stored in session storage. The extension can read QR images, canvas elements, and SVG elements, and can attempt a visible-tab screenshot as a fallback.

Cross-origin QR images may be fetched from their source without cookies. There is no external payment-analysis API in the bundled implementation.

The extension requests `storage`, `scripting`, and access to all URLs so it can inspect checkout pages. Chrome's protected internal pages are not supported.

## Current limitations

- Warnings are advisory. The extension does not sign, send, reverse, or block wallet transactions.
- If an attacker changes both the displayed payment details and the QR consistently, this version may not detect the attack.
- **Merchant-signed payment intent verification is not implemented in the bundled v1.5.1 source.** This archive verifies checkout/QR consistency and local registry matches.
- Token checking is limited. This version detects a QR requesting an SPL token when the page states SOL; it does not comprehensively validate every token identity.
- QR decoding and text extraction depend on page structure, visibility, and image readability.
- The bundled interface is in Portuguese.

## Videos

- [Product demo](https://www.youtube.com/watch?v=WhS2cwV6r8g)
- [Project pitch](https://youtu.be/qQkiolOnlrY)

## Third-party license

The QR-enabled archive includes `jsQR.js` by Cosmo Wolfe, licensed under Apache License 2.0. Its license is included as `jsQR-LICENSE.txt`.
