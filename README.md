## Dockerized React App (Live Reload Enabled)
This project demonstrates how to run a React application inside Docker with live code reloading.
Any changes made inside src/App.tsx (or any file in src/) will automatically refresh in the browser.

# Features:

⦁	Fully Dockerized React app.

⦁	Live reload using bind mounts.

⦁	Auto-forwarded port (via Docker Desktop + VS Code WSL)

⦁	No need for docker-compose, single Dockerfile.

⦁	Works inside WSL2 Ubuntu environment.

⦁	Development Mode (Live Reload).

⦁	React app runs inside Docker, but your local source code is mounted for instant refresh.

 
                ┌───────────────────────────────┐
                │        Local Machine           │
                │  (VS Code + WSL2 + Docker)     │
                └───────────────────────────────┘
                               │
                               │ Edit Code (src/, public/)
                               ▼
          ┌──────────────────────────────────────────────────┐
          │               Development Mode                    │
          │              (Dockerfile.dev)                     │
          └──────────────────────────────────────────────────┘
                               │
                               │ docker run -v src:/app/src
                               ▼
                ┌───────────────────────────────┐
                │     Node.js Dev Server        │
                │    Runs: npm start            │
                │    React Fast Refresh         │
                └───────────────────────────────┘
                               │
                               ▼
                   http://localhost:3000
                 (Live Reload / Fast Refresh)



──────────────────────────────────────────────────────────────────

          ┌──────────────────────────────────────────────────┐
          │                  Production Mode                  │
          │                   (Dockerfile)                    │
          └──────────────────────────────────────────────────┘
                               │
                               │ docker build → build folder
                               ▼
                ┌───────────────────────────────┐
                │        React Build Files       │
                │        (/app/build)            │
                └───────────────────────────────┘
                               │
                               │ COPY build → nginx
                               ▼
                ┌───────────────────────────────┐
                │             Nginx              │
                │ Serves static HTML/CSS/JS      │
                │ From: /usr/share/nginx/html    │
                └───────────────────────────────┘
                               │
                               ▼
                   http://localhost:3000
                    (Optimized Production UI)

# Project Structure
react-app/
│
├── Dockerfile      Production build (Nginx)
├── Dockerfile.dev  Development mode with live reload
├── package.json
├── package-lock.json
├── public/
└── src/
    ├── App.tsx
    ├── index.tsx

# 1. Create the React + TypeScript Project

$mkdir react-app

$cd react-app

$npx create-react-app --template typescript react-app

# Move into the generated folder:

$cd react-app

# Start locally to verify:

$npm start

# Build the React app:

$npm run build

# test build output locally:

$npx http-server@14.1.1 build

# 2. Build the Dockerfile:

Dockerfile.dev (Used for Development)

Dockerfile  (Production)

# Build Production Image

$docker build -t react-app:v1 .

# Run Production Container

$docker run --rm -d -p 3000:80 react-app:v1

Access:
👉 http://localhost:3000

# 3. Dockerfile.dev (Development with Live Reload)

# Build Dev Image

$docker build -t react-app:v2 -f Dockerfile.dev .

# Run with Live Reload (Important):

$docker run --rm -d \
  -p 3000:3000 \
  -v ./public:/app/public \
  -v ./src:/app/src \
  react-app:v2

Access: 👉 http://localhost:3000

# When you edit src/App.tsx, the browser refreshes automatically.






