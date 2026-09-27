<div align="center">

  <h1>🔗 Chat URL Shorter</h1>
  <p><strong>A modern, high-performance single-file URL shortening engine with instant QR generation, smart deep-link routing, and real-time Firebase click analytics.</strong></p>

  <p>
    <a href="https://shortlink-rosy-chi.vercel.app/"><img src="https://img.shields.io/badge/Live_Demo-Visit_App-6366f1?style=for-the-badge&logo=vercel&logoColor=white" alt="Live Demo" /></a>
    <img src="https://img.shields.io/badge/License-MIT-emerald?style=for-the-badge" alt="License" />
    <img src="https://img.shields.io/badge/Tailwind_CSS-38B2AC?style=for-the-badge&logo=tailwind-css&logoColor=white" alt="Tailwind CSS" />
    <img src="https://img.shields.io/badge/Firebase_RTDB-FFCA28?style=for-the-badge&logo=firebase&logoColor=black" alt="Firebase RTDB" />
  </p>

  <p>
    <a href="https://shortlink-rosy-chi.vercel.app/">🌐 <strong>Try the Live Application</strong></a>
  </p>

</div>

---

## 📖 Overview

**Chat URL Shorter** is an ultra-fast, serverless, single-file URL shortening web application. Built with a sleek Deep Indigo and Slate glassmorphism aesthetic, it turns lengthy, cluttered URLs into clean, compact links (`domain.com/:shortCode`) with zero server overhead.

Featuring persistent **Firebase Realtime Database** storage, vector **QR code export**, platform auto-detection (YouTube, Instagram, Maps, GitHub, etc.), and real-time click tracking, it provides a complete link management suite directly in the browser.

---

## ⚡ How It Works

```
Long URL Input  ──►  Base62 Generator & Collision Check  ──►  Firebase RTDB (Atomic set)
                                                                       │
Visitor opens /:shortCode  ◄──  SPA Rewrite (vercel.json)  ─────────────┘
          │
          ├──► 1. Parse pathname cleanly (/:shortCode)
          ├──► 2. Firebase atomic click increment & timestamp update
          └──► 3. Instant browser redirect via window.location.replace()
```

1. **Collision-Safe Base62 Generation**: 
   When shortening a link, the system generates a 6-character Base62 string (`0-9`, `a-z`, `A-Z`) and performs a real-time existence check in Firebase before persisting. If a custom alias is requested, it is strictly validated against reserved keywords and sanitized.
2. **Clean SPA Path Routing**: 
   Through `vercel.json` wildcard rewrites (`/(.*) -> /index.html`), all clean short URL paths load `index.html`. The client-side router extracts the slug directly from `window.location.pathname`, bypassing traditional 404 errors.
3. **Atomic Analytics & Safe Redirects**:
   On link access, Firebase Realtime Database runs an atomic transaction (`clicks + 1`) and logs `lastClickedAt` asynchronously while instantly executing `window.location.replace()` to prevent redirect delay and back-button traps.

---

## ✨ Key Features

- 💎 **Deep Indigo Glassmorphism UI**: Translucent backdrop blur cards, subtle borders, responsive layout, and pure SVG iconography.
- 📱 **Clean Path URLs**: Fully supports clean domain routes (`https://shortlink-rosy-chi.vercel.app/my-link`) without hash fragments.
- 🎯 **Smart Destination Recognition**: Automatically recognizes platforms like YouTube, Instagram, TikTok, Facebook, X/Twitter, Reddit, WhatsApp, Google Maps, Spotify, and GitHub.
- 📲 **Vector QR Code Generator**: Generates crisp, downloadable PNG/SVG QR codes formatted for direct mobile camera scanning.
- 📊 **Visual Performance Analytics**: Live dashboard showcasing total clicks, platform distribution bars, and top-performing links.
- 🛡️ **Client Workspace Sandboxing**: Generates an anonymous device UUID in `localStorage` so users can manage, disable, or delete only the links they created.
- 🌓 **Theme Preference**: One-click switching between Dark, Light, and System modes.

---

## 🛠️ Tech Stack

| Technology | Purpose |
| :--- | :--- |
| **HTML5 & CSS3** | Single-file architecture & semantic document structure |
| **Tailwind CSS (CDN)** | Responsive layout, modern utility styling & glassmorphism |
| **JavaScript (ES6+)** | Base62 generation, validation, state management, and SPA routing |
| **Firebase Realtime Database (v9)** | Cloud persistence, atomic transactions, and real-time syncing |
| **QRCode.js** | Client-side vector QR code rendering and image export |
| **Vercel** | Edge hosting with single-page application wildcard rewrites |

---

## 🚀 Setup & Local Configuration

### 1. Clone the Repository

```bash
git clone https://github.com/your-username/chat-url-shorter.git
cd chat-url-shorter
```

### 2. Configure Your Firebase Project

Open `index.html` and update the `firebaseConfig` object with your own Firebase project credentials:

```javascript
const firebaseConfig = {
  apiKey: "YOUR_API_KEY",
  authDomain: "YOUR_PROJECT_ID.firebaseapp.com",
  databaseURL: "https://YOUR_PROJECT_ID-default-rtdb.firebaseio.com",
  projectId: "YOUR_PROJECT_ID",
  storageBucket: "YOUR_PROJECT_ID.firebasestorage.app",
  messagingSenderId: "YOUR_SENDER_ID",
  appId: "YOUR_APP_ID"
};
```

### 3. Configure Firebase Realtime Database Security Rules

To ensure query performance when indexing links by owner identity, set the following rules in your **Firebase Console → Realtime Database → Rules**:

```json
{
  "rules": {
    "links": {
      ".read": true,
      ".write": true,
      ".indexOn": ["ownerId"]
    }
  }
}
```

### 4. Run Locally

You can serve `index.html` using any local development server:

```bash
# Using Python
python3 -m http.server 3000

# Using Node / npx
npx serve .
```

Visit `http://localhost:3000` in your browser.

---

## 📦 Deployment on Vercel

This repository includes `vercel.json` preconfigured for clean path SPA rewrites:

```json
{
  "rewrites": [
    {
      "source": "/(.*)",
      "destination": "/index.html"
    }
  ]
}
```

Simply import the repository into [Vercel](https://vercel.com) and click **Deploy**. Clean URLs like `https://your-domain.vercel.app/:shortCode` will work out of the box.

---

## 📄 License

Distributed under the MIT License. See [`LICENSE`](./LICENSE) for more information.
