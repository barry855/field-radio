# WebRTC Two-Way Radio
## Files
- `ALPHA.html` — Unit Alpha
- `BRAVO.html` — Unit Bravo

## Requirements
- Chrome, Edge or Chromium-based browser with microphone access
- Both pages must be served over HTTPS **or** opened from `localhost`
- An internet connection (uses STUN/TURN servers for NAT traversal)

## Quick Start

**Option A — Same machine, two tabs:**
1. Open `ALPHA.html` in one browser tab
2. Open `BRAVO.html` in another tab or window
3. Enter the same session code on both units
4. Alpha will call Bravo automatically once both are online

**Option B — Two different computers on the same network:**
1. Serve the files from a local server, e.g.:
   ```
   npx serve .
   ```
   or use VS Code Live Server, Python's `http.server`, etc.
2. Open Alpha on one machine, Bravo on the other
3. Enter the same session code on both

**Option C — Two different people, anywhere:**
Deploy `ALPHA.html` and `BRAVO.html` to any static hosting service (Netlify, GitHub Pages, Vercel).
Both must be on HTTPS. Share the session code with the other person via any out-of-band channel.

## Session Codes
- Enter the same code on Alpha and Bravo to connect them
- Codes can be any alphanumeric string up to 8 characters
- Presence data written to Firebase is AES-GCM encrypted using a key derived from the session code — anyone observing the signalling channel without the code cannot determine peer identities

## Controls

| Control | Function |
|---------|----------|
| Session code | Shared secret — both units must use the same code |
| Pi Mode (compact) | Hides speech-to-text controls — use on Raspberry Pi or any device without STT support |
| Disable Video | Join without camera |
| Disable Mike | Join without microphone (data/chat only) |
| Talk/Type in Chat | Opens the speech and text input panel |
| TALK | Hold to dictate via speech-to-text |
| SEND | Send typed or dictated text to chat |
| ATTACH | Attach a file to send (drag and drop also supported) |
| CW | Send input box text as Morse code audio |
| TTS | Toggle auto-speak for incoming messages |
| ORIGINAL | Reset speech language to system default |
| ↺ | Refresh / reconnect |
| ✕ Leave | End the session |
| RECORD | Record the current call |
| MINI CAM | Toggle picture-in-picture camera |
| FS | Toggle full screen |
| SYS LOG | Toggle the system diagnostic log |

## Status Indicators

| LED colour | Meaning |
|------------|---------|
| Off (black) | Not yet connected to signalling |
| Yellow / blinking | CALLING — dialling the remote unit |
| Blue / blinking | CONNECTING — ICE negotiation in progress |
| Amber / solid | WAITING — remote unit not yet online |
| Green / solid | LIVE — full duplex active |

## Voice Commands (Speech Input)
Say commands clearly while dictating. See `FieldRadio_VoiceCommands.pdf` for the full list in all supported languages. English examples:

| Say | Result |
|-----|--------|
| full stop / period | . |
| comma | , |
| question mark | ? |
| new line | ↵ |
| new paragraph | ↵↵ |
| scratch that | deletes last sentence |
| delete word | deletes last word |
| clear everything | clears the input box |

Supported languages: English (US/UK), Español, Français, Deutsch, Português (BR).

## Chat History
- Chat is saved automatically when you press Leave
- Up to 5 previous sessions per code are stored in browser local storage
- Use **RESTORE** to load a previous session — a picker appears if more than one exists

## Raspberry Pi / ChromeOS
Alpha and Bravo run on Raspberry Pi OS (which reports as `CrOS x86_64` in the browser user agent). Enable **Pi Mode (compact)** on the login screen to hide speech-to-text controls that are not supported on that platform. The setting is stored per-device.

## How It Works
- **Signalling**: PeerJS v1.5.5 over the public PeerJS cloud
- **Presence**: Firebase Realtime Database (peer IDs encrypted with AES-GCM, key derived from session code via PBKDF2)
- **Media**: WebRTC `getUserMedia` for audio and video; data channel for chat, file transfer, and Morse code
- **TURN**: Metered.ca TURN relay for NAT traversal where direct connection is not possible
- **STT**: Web Speech API (`webkitSpeechRecognition`) with localised voice command maps
- **TTS**: Web Speech Synthesis API

## Troubleshooting

| Problem | Fix |
|---------|-----|
| Black LED, no status | Session code already registered on signalling server — wait ~10 s for automatic fallback to a suffixed ID |
| WAITING indefinitely | Other unit is not online with the same code, or Firebase presence is stale — try leaving and rejoining |
| No audio after connecting | Check system volume and audio output device; try the ↺ reconnect button |
| Mic access denied | Allow microphone in browser permissions and reload |
| TALK button missing | Check that Pi Mode is not enabled, and that the browser supports `webkitSpeechRecognition` (Chrome/Edge only) |
| TTS row missing | Browser has no speech synthesis voices installed |
| Tooltips on Android | Long-press any button to see its tooltip |
| Files appear doubled on drop | Ensure you are dropping onto the speech panel, not the chat area |
