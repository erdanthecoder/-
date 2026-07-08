# NexChat

A WhatsApp-style messenger: 1:1 and group chats, image/file sharing, and WebRTC voice/video calls (including group calls). UI available in English and Russian.

## Features

- **Accounts** — register with username + password, sessions persist across visits via a secure cookie (stays logged in on that device).
- **Direct & group chats** — real-time messaging over Socket.io, typing indicators, online/last-seen presence, unread counts.
- **Groups** — create groups, add/remove members, admin roles, group avatar/name.
- **Media sharing** — send images and files (PDF, docs, zip, txt) in any chat.
- **Calls** — WebRTC voice and video calls, 1:1 and full-mesh group calls, signaled over Socket.io (STUN only, no external TURN service configured).
- **Languages** — English and Russian, switchable from the login screen or inside the app; each user's choice is saved to their profile.

## Running locally

```bash
npm install
npm start
```

Then open **http://localhost:3000**.

Data is stored in a local SQLite database at `data/nexchat.db` (created automatically). Uploaded images/files are stored in `uploads/`. Both are gitignored — safe to delete to reset all data.

## Tech stack

- **Backend**: Node.js, Express, Socket.io, better-sqlite3, JWT (httpOnly cookie sessions), bcryptjs, multer (uploads)
- **Frontend**: Vanilla HTML/CSS/JS (no build step), native WebRTC APIs

## Project layout

```
server.js          Express app + Socket.io (realtime messaging, presence, WebRTC signaling)
db.js               SQLite schema/connection
auth.js              JWT cookie session helpers
routes/
  auth.js            register/login/logout/me/profile/user search
  chats.js            conversations, groups, members, message history
  upload.js           image/file upload endpoint
public/
  index.html          app shell (auth screen, chat UI, modals, call overlay)
  css/style.css        WhatsApp-style UI, light + dark theme
  js/i18n.js            EN/RU translation dictionary
  js/api.js             REST client
  js/webrtc.js           WebRTC call manager (mesh signaling for groups)
  js/app.js              UI logic, state, socket wiring
```

## Notes on calls

Calls use only public STUN servers for NAT traversal. On restrictive corporate/mobile networks that block peer-to-peer UDP, a TURN relay would be needed — none is configured here since it typically requires a paid or self-hosted relay service.
