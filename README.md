# rCapture

rCapture is a single-page web app that adapts capture behavior by context:

- On likely mobile devices, it offers live camera preview, image capture, and in-browser OCR text extraction.
- On desktop/web contexts, it provides image file upload, instant preview, and in-browser OCR text extraction.

## Features

- **Adaptive mode selection**
  - Capability-first auto-detect (checks `getUserMedia` support, touch pointer, UA, and viewport).
  - Camera-first on likely mobile devices and upload-first on desktop contexts.

- **Mobile camera + microphone flow**
  - Permissions requested only on explicit button tap (reliable on Safari and Chrome mobile).
  - Prefers rear-facing camera (`facingMode: environment`) with automatic front-camera fallback.
  - Live camera feed rendered in a `<video>` element with `playsinline` and `muted` for autoplay stability.
  - Clear in-page error messages for: permission denied, device not found, device in use, insecure origin.

- **Image capture flow**
  - Capture Image button appears once the stream is active.
  - Snapshots the current video frame onto an off-screen `<canvas>` and exports as PNG.
  - Captured image displayed below the live feed for review.
  - OCR starts automatically after capture.
  - Capture Another resets the captured image and OCR output for the next receipt.

- **Desktop upload flow**
  - `<input type="file" accept="image/*">` for selecting local image files.
  - Instant in-page preview using `URL.createObjectURL()`.
  - OCR starts automatically after a valid image is selected.
  - Validates file type before preview; rejects non-image files with an error message.

- **OCR text extraction (both flows)**
  - Powered by [Tesseract.js v5](https://github.com/naptha/tesseract.js) — runs entirely in the browser, no server or API key needed.
  - Mobile: OCR runs immediately after capture and stops the live stream before processing.
  - Desktop: OCR runs immediately after an image file is loaded.
  - Live progress percentage shown while the OCR engine processes the image.
  - Extracted text displayed in a shared scrollable monospace panel with a Copy Text action.
  - OCR engine (~10 MB) is downloaded on first use and cached by the browser.
  - Accuracy depends on image clarity — works best on clear, straight, well-lit receipt photos.

- **Session safety and resource cleanup**
  - Active media tracks stopped on page hide and `pagehide`.
  - Object URLs revoked on file change and page unload to prevent memory leaks.

## Tech Stack

- Plain HTML, CSS, and vanilla JavaScript
- No frameworks
- [Tesseract.js v5](https://github.com/naptha/tesseract.js) via CDN for in-browser OCR
- Single file app entry: `index.html`

## How to Run

1. Open `index.html` directly in a browser for basic UI testing.
2. For camera/microphone access, serve over `localhost` or HTTPS.

### Quick local server options

Python 3:

```bash
python3 -m http.server 8080
```

Then open:

- `http://localhost:8080`

## Permissions and Browser Notes

- Camera/microphone APIs require secure contexts in most browsers:
  - HTTPS, or
  - `localhost`
- Some mobile browsers require explicit user interaction before prompting permissions.
- If permissions are denied, the app shows recovery guidance in the status area.

## Live App

https://receipt-image.vercel.app/

## File Structure

- `index.html` — app UI, styling, and all runtime logic
- `flyer.html` — printable/shareable flyer with QR code and usage instructions
- `README.md` — project overview and usage guidance
