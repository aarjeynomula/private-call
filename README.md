# Private Call — SIM-free WebRTC prototype

This is a starter voice-calling web app for two Android phones. It uses:
- GitHub Pages (HTTPS hosting)
- Firebase Authentication (anonymous sign-in)
- Firebase Realtime Database (signalling)
- WebRTC (voice media)

## Important
This is a prototype, not a production-secure calling service. The six-digit room code is a shared room key, not a verified personal identity. Do not use it for sensitive calls. For reliable calls across restrictive mobile/carrier networks, configure a TURN server; public STUN alone is not enough.

## 1. Create Firebase project
1. Go to https://console.firebase.google.com/ and create a project.
2. Add a Web app and copy its Firebase config.
3. In Authentication > Sign-in method, enable **Anonymous**.
4. In Realtime Database, create a database.
5. Paste the web config values into `firebaseConfig` in `index.html`.

## 2. Realtime Database rules (prototype only)
For an initial two-phone test, use the following rules. They allow authenticated anonymous users to read/write room data. This is convenient for testing but not sufficient for production privacy; anyone who knows a room code may be able to access it.

```json
{
  "rules": {
    ".read": "auth != null",
    ".write": "auth != null"
  }
}
```

For production, replace these with room-membership validation, short-lived room tokens, rate limits, cleanup and abuse protection. Do not leave broad database rules enabled for a public deployment.

## 3. Test locally
Because microphone access requires a secure context, use HTTPS hosting or `localhost`. Open `index.html` on a local development server, not as a `file://` URL.

## 4. Publish on GitHub Pages
1. Create a private GitHub repository (or public if you are comfortable publishing the frontend source).
2. Upload `index.html` and `README.md`.
3. Repository Settings > Pages > Deploy from branch > `main` / root.
4. Open the generated `https://...github.io/...` URL on both Android phones.

GitHub Pages sites are public URLs even if your source repo is private on eligible plans. Do not put service-account keys or secrets in the HTML. Firebase web config is not a server secret; database rules enforce access.

## 5. Call test
1. Open the same HTTPS URL on both phones.
2. Allow microphone permission.
3. Enter the same six-digit room code on both phones and tap Connect.
4. On one phone tap Call.
5. On the other phone tap Answer.
6. Test with headphones first to reduce echo.

## Current prototype limitations
- No background incoming-call notification when the browser is closed or suspended.
- Keep both browser tabs open and screens awake during the test.
- The prototype is voice-only.
- Call cleanup is basic; use a fresh six-digit room code for each test.
- The code does not implement a secure invite/identity system.
- TURN server support and production database security rules still need to be added.
