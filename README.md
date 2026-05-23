# WebRTC Two-Way Field Radio

## Files
- `radio-alpha.html` — Unit Alpha (olive/amber theme)
- `radio-bravo.html` — Unit Bravo (steel/cyan theme)

## How to Use

### Requirements
- A modern browser (Chrome, Firefox, Edge) with microphone access
- Both pages must be served over HTTPS **or** opened from `localhost`
  (WebRTC + getUserMedia require a secure context)
- An internet connection (uses Google STUN servers for NAT traversal)

### Quick Start

**Option A — Same machine, two tabs:**
1. Open `radio-alpha.html` in one browser tab
2. Open `radio-bravo.html` in another tab (or another browser window)
3. Copy the **YOUR ID** shown on Alpha
4. Paste it into the **Remote Peer ID** field on Bravo, then click **ESTABLISH LINK**
5. Grant microphone permission when prompted
6. Once linked, hold **PUSH TO TALK** on either unit to transmit

**Option B — Two different computers on the same network:**
1. Serve the files from a local server, e.g.:
   ```
   npx serve .
   ```
   or use VS Code Live Server, Python's `http.server`, etc.
2. Open Alpha on machine 1, Bravo on machine 2
3. Follow the same ID-copy-paste steps above

**Option C — Two different people, anywhere:**
Deploy the HTML files to any static hosting service (Netlify, GitHub Pages, Vercel).
Both must be on HTTPS. Share your peer ID with the other person via any channel.

### Controls

| Control | Function |
|---------|----------|
| VOL knob | Visual only (drag to rotate) |
| SQ knob | Visual only (drag to rotate) |
| CH-01 – CH-04 | Channel selector (display only) |
| ESTABLISH LINK | Initiates or answers a WebRTC call |
| PUSH TO TALK | Hold to transmit your microphone audio |

### How It Works

- Uses **PeerJS** (v1.5.4) as a WebRTC signalling + peer management layer
- Audio is muted by default; holding PTT enables your mic track
- The waveform display shows incoming audio (green) or your outgoing transmission (red/cyan)
- Signal meter bars animate while a call is active
- The LED indicators show: connection state, TX (transmitting), RX (receiving)

### Troubleshooting

| Problem | Fix |
|---------|-----|
| "MIC ACCESS DENIED" | Allow microphone in browser permissions |
| ID shows "INITIALIZING..." | Reload — PeerJS server may be slow |
| Can't connect | Ensure both pages are on HTTPS or localhost |
| No audio after connecting | Check your system volume and audio output device |
