# Time Accurate

A web clock that syncs with a Cloudflare Worker to stay accurate to server time, rather than relying on the client's clock.

## Features

- Server-synced time via Cloudflare Worker
- Dark mode UI with large, modern display
- 12h / 24h format toggle
- Displays sync offset in milliseconds

## Setup

### Part 1: Deploy Cloudflare Worker

1. **Install Wrangler** (if not already installed):
   ```bash
   npm install -g wrangler
   ```

2. **Login to Cloudflare**:
   ```bash
   wrangler login
   ```

3. **Create a new Worker**:
   ```bash
   wrangler init time-worker
   ```
   - Select "JavaScript" as the type
   - When asked to modify `wrangler.toml`, say no (we'll use the default)

4. **Copy the worker code**:
   Copy the contents of `worker.js` in this repo into your worker's `src/index.js` (or just replace the worker file created by wrangler).

5. **Deploy**:
   ```bash
   wrangler deploy
   ```

6. **Note your worker URL**:
   It will look like: `https://time-worker.your-username.workers.dev`

### Part 2: Update Frontend

1. Open `index.html` in a text editor
2. Find this line (near the bottom):
   ```javascript
   const response = await fetch('https://YOUR-WORKER-SUBDOMAIN.workers.dev/time');
   ```
3. Replace the URL with your actual worker URL:
   ```javascript
   const response = await fetch('https://time-worker.your-username.workers.dev/time');
   ```

### Part 3: Deploy to GitHub Pages

1. **Create a GitHub repository**:
   - Go to https://github.com/new
   - Name it `time-accurate` (or whatever you prefer)
   - Do NOT initialize with README (we already have one)

2. **Push your code**:
   ```bash
   git init
   git add .
   git commit -m "Initial commit"
   git branch -M main
   git remote add origin https://github.com/your-username/time-accurate.git
   git push -u origin main
   ```

3. **Enable GitHub Pages**:
   - Go to your repository on GitHub
   - Click **Settings** → **Pages** (left sidebar)
   - Under **Build and deployment**:
     - Source: **Deploy from a branch**
     - Branch: **main** → **/(root)**
     - Click **Save**
   - Wait 1-2 minutes, then your site will be live at:
     `https://your-username.github.io/time-accurate/`

## How It Works

1. On page load, the frontend fetches the current time from the Cloudflare Worker
2. It calculates the network round-trip time and adjusts for latency
3. The offset between your computer's clock and the server is computed
4. `setInterval` updates the display every second using `Date.now() + offset`
5. This keeps the displayed time accurate to the server even if your system clock is wrong

## Files

- `worker.js` - Cloudflare Worker that returns server time
- `index.html` - Frontend clock UI and sync logic
- `README.md` - This file
