# rCapture

rCapture is a single-page web app that adapts capture behavior by context:

- On likely mobile devices, it offers live camera preview with microphone permission support.
- On desktop/web contexts, it provides image file upload with instant preview.

## Features

- **Adaptive mode selection**
  - Capability-first auto-detect (checks `getUserMedia` support, touch pointer, UA, and viewport).
  - Manual override buttons: Auto Detect, Use Camera, Use Upload.
  - Selected mode persists across sessions via `localStorage`.

- **Mobile camera + microphone flow**
  - Permissions requested only on explicit button tap (reliable on Safari and Chrome mobile).
  - Prefers rear-facing camera (`facingMode: environment`) with automatic front-camera fallback.
  - Live camera feed rendered in a `<video>` element with `playsinline` and `muted` for autoplay stability.
  - Clear in-page error messages for: permission denied, device not found, device in use, insecure origin.

- **Image capture and download**
  - Capture Image button appears once the stream is active.
  - Snapshots the current video frame onto an off-screen `<canvas>` and exports as PNG.
  - Captured image displayed below the live feed for review.
  - Download Image link saves the PNG directly to the device.
  - Clear Snapshot button dismisses the captured image.

- **Desktop upload flow**
  - `<input type="file" accept="image/*">` for selecting local image files.
  - Instant in-page preview using `URL.createObjectURL()`.
  - Validates file type before preview; rejects non-image files with an error message.

- **Session safety and resource cleanup**
  - Active media tracks stopped on page hide, `pagehide`, and mode switch away from camera.
  - Object URLs revoked on file change and page unload to prevent memory leaks.

## Tech Stack

- Plain HTML, CSS, and vanilla JavaScript
- No frameworks
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
