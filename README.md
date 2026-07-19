# 🏀 Buzzer Beater
 
**Buzzer Beater** is a French-language web platform dedicated to the NBA (National Basketball Association). It brings together news, team profiles, an interactive map of basketball courts, and a community forum in a single place — built as a school project (APP3 — Web Programming module, 2025-2026).
 
> The name *"Buzzer Beater"* refers to a shot scored at the very last second — a nod to the passion the project aims to share.
 
## ✨ Features
 
- 🏠 **Home page** with responsive navigation (hamburger menu on mobile), hero section, feature cards, and a featured team carousel
- 📰 **NBA News** — dynamically fetched from the RMC Sport RSS feed and parsed client-side
- 📖 **Team Encyclopedia** — profiles for all 30 NBA teams (location, history, current roster) with a parallax background effect
- 🔐 **Authentication** — registration with real-time validation and login, backed by a REST API with hashed passwords
- 🗺️ **Interactive Map** — a React + Mapbox module for browsing and adding basketball courts ("pins") with ratings, protected behind authentication
- 💬 **Community Forum** — posts, threaded replies, and deletion, with a REST API and XSS-safe rendering
- 🔄 **Unified session management** across the static site and the React app, despite running on two different local origins
## 🛠️ Tech Stack
 
### Main website (static front-end)
| Technology | Purpose |
|---|---|
| HTML | Page structure (`bb_menu.html`, `bb_news.html`, `bb_teams.html`, `bb_forum.html`, `login.html`, `register.html`) |
| CSS | Single consolidated stylesheet (`style_teams.css`) — CSS variables, glassmorphism, parallax effects, responsive design (no CSS framework) |
| JavaScript (vanilla) | RSS parsing (`DOMParser`), form validation, session handling, `fetch` calls to the API |
| Font Awesome (CDN) | Icons |
 
### Back-end API (`map_bb/backend_map`)
| Technology | Version | Purpose |
|---|---|---|
| Node.js | — *(not pinned in the repo)* | JavaScript server runtime |
| Express | ^5.2.1 | REST API framework |
| MongoDB Atlas | driver `mongodb` ^7.0.0 | Hosted NoSQL database *(cloud-hosted, so no local server version to pin)* |
| Mongoose | ^9.1.5 | MongoDB ODM / schemas |
| bcryptjs | ^3.0.3 | Password hashing |
| cors | ^2.8.6 | Cross-origin request handling |
| dotenv | ^17.2.3 | Environment variable management |
| nodemon | 3.1.11 | Dev auto-reload |
 
### Map module (`map_bb/frontend_map`)
| Technology | Version | Purpose |
|---|---|---|
| React | ^19.2.4 | UI library |
| Vite | ^7.2.4 | Build tool / dev server |
| Mapbox GL / react-map-gl | ^3.18.1 / ^8.1.0 | Interactive mapping |
| Material UI (MUI) | ^7.3.7 | Ready-made form/popup components |
| Axios | ^1.13.5 | HTTP client for API calls |
 
## 📁 Project Structure
 
```
Buzzer_Beater/
├── pgs_prjts/               # Static site HTML pages
│   ├── bb_menu.html         # Home page
│   ├── bb_news.html         # NBA news (RSS)
│   ├── bb_teams.html        # 30-team encyclopedia
│   ├── bb_forum.html        # Community forum
│   ├── login.html           # Login (wired to the API)
│   └── register.html        # Registration (wired to the API)
├── imgs_prjt/               # Images and visual assets
│   └── Parallax/            # NBA team background images
├── old_css/                 # Legacy per-page stylesheets (superseded by style_teams.css)
├── navbar-auth.js           # Shared auth/session logic across pages
├── style_teams.css          # Main stylesheet, used by every page
├── map_bb/
│   ├── backend_map/         # Express REST API
│   │   ├── index.js
│   │   ├── models/          # User, Pin, Post (Mongoose schemas)
│   │   └── routes/          # users, pins, forum
│   └── frontend_map/        # React + Vite map app
│       └── src/
│           ├── App.jsx
│           └── components/  # Login, Register
└── cmd_bdd                  # Quick-start commands
```
 
> **Note:** the `old_css/` folder contains earlier, per-page stylesheets (`style_login.css`, `style_menu.css`, `style_news.css`) that have since been merged into the single `style_teams.css` now referenced by all pages. They're kept in the repo but no longer used.
 
## ⚙️ Getting Started
 
The project runs entirely locally with two servers: the Express API/static site (port 3000) and the React map app (port 5173).
 
### 1. Back-end API
```bash
cd map_bb/backend_map
npm install
# create a .env file with: Mongo_Url=mongodb+srv://...
node index.js
# → MongoDB connected!
# → Backend server is running! (port 3000)
```
 
### 2. Map front-end
```bash
cd map_bb/frontend_map
npm install
# create a .env file with: VITE_MAPBOX=pk.eyJ1...
npm run dev
# → VITE ready at http://localhost:5173/
```
 
### 3. Access the site
The main static site is served by the back-end at `http://localhost:3000/pgs_prjts/bb_menu.html`.
 
| URL | Page |
|---|---|
| `http://localhost:3000/pgs_prjts/bb_menu.html` | Home |
| `http://localhost:3000/pgs_prjts/login.html` | Login |
| `http://localhost:3000/pgs_prjts/register.html` | Register |
| `http://localhost:3000/pgs_prjts/bb_forum.html` | Forum |
| `http://localhost:5173` | Interactive map |
 
## 🗄️ Data Models (MongoDB)
 
**User** — `username`, `email`, `password` (bcrypt-hashed), timestamps
**Pin** (basketball court) — `username`, `title`, `desc`, `rating` (0-5), `lat`, `long`, timestamps
**Post** (forum) — `username`, `title`, `content`, embedded `replies[]`, timestamps
 
## 🔐 Session Handling Across Two Origins
 
Since the static site (`localhost:3000`) and the React app (`localhost:5173`) don't share `localStorage`, the session is synchronized by passing a `?user=USERNAME` URL parameter between them (and `?logout=1` on sign-out), read on each side by `navbar-auth.js` (static site) or React state (map app).
 
## 👥 Authors
 
Project built by **Yanis Metari** ([Yanis-LM5](https://github.com/Yanis-LM5)) and **Christopher Dindanda Matondo** ([ChrichriSama](https://github.com/ChrichriSama)), as part of the APP3 Web Programming module (2025-2026).
 
## 📄 License
 
No license is currently defined for this project.