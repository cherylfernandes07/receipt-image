# rCapture

rCapture is a single-page web app that adapts capture behavior by context:

- On likely mobile devices, it offers live camera preview with microphone permission support.
- On desktop/web contexts, it provides image file upload with instant preview.

## Features

- Adaptive mode selection:
  - Auto-detect mode based on browser capabilities and device heuristics.
  - Manual override controls: Auto Detect, Use Camera, Use Upload.
- Mobile camera + microphone flow:
  - Requests `getUserMedia({ video, audio })` on user tap.
  - Shows live camera feed in-page.
  - Handles common permission and device errors with user-friendly messages.
- Desktop upload flow:
  - Accepts image files via `<input type="file" accept="image/*">`.
  - Previews selected image immediately in-page.
- Session safety and cleanup:
  - Stops active media tracks when hidden or page is closed.
  - Revokes object URLs to avoid memory leaks.
- Preference persistence:
  - Saves selected mode (`auto`, `camera`, `upload`) in `localStorage`.

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

## Current UX Copy

- App name: rCapture
- Header tagline: rCapture - mobile or desktop
- Camera action: Start Live Scan
- Upload mode: image preview

## File Structure

- `index.html` - app UI, styling, and all runtime logic
- `README.md` - project overview and usage guidance
