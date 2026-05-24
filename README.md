# UNDERGROUND — Setup Guide

A terminal-only social app. Channels, DMs, anonymous posting, file sharing.
No browser. Runs in CMD or PowerShell as a .exe.

---

## PART 1 — Deploy the Server on Railway (free)

Railway hosts your backend. Free plan is enough.

### Step 1: Create a GitHub repo for the server

1. Go to https://github.com and create a new repo called `underground-server`
2. Upload everything inside the `/server` folder to that repo:
   - index.js
   - package.json
   - railway.toml
   - .gitignore

### Step 2: Deploy on Railway

1. Go to https://railway.app and sign up (free)
2. Click **New Project** → **Deploy from GitHub repo**
3. Select your `underground-server` repo
4. Railway auto-detects Node.js and deploys it

### Step 3: Add environment variables on Railway

In your Railway project dashboard → **Variables**, add:

| Variable     | Value                          |
|--------------|-------------------------------|
| JWT_SECRET   | any long random string        |
| BASE_URL     | https://your-app.railway.app  |

> For JWT_SECRET, just mash your keyboard: `xK9!mQ2pZ...` — anything long and random.
> For BASE_URL, use the URL Railway gives you (shown in the Deployments tab).

### Step 4: Get your server URL

In Railway → your project → **Settings** → **Domains**
It looks like: `underground-server-production-xxxx.up.railway.app`

Copy that URL — you need it in the next part.

---

## PART 2 — Set Up the Client

### Step 1: Edit underground.py

Open `client/underground.py` in Notepad.
Find line 18:

```python
SERVER_URL = "https://YOUR_RAILWAY_URL_HERE"
```

Replace it with your Railway URL:

```python
SERVER_URL = "https://underground-server-production-xxxx.up.railway.app"
```

Save the file.

### Step 2: Build the .exe

You need Python installed: https://python.org/downloads
Make sure to check "Add Python to PATH" during install.

Then:
1. Open CMD in the `client/` folder
2. Run: `build.bat`
3. Wait ~1 minute
4. Your `.exe` appears in `client/dist/underground.exe`

### Step 3: Run it

Double-click `underground.exe` — or run it from CMD:

```
underground.exe
```

---

## HOW TO USE

```
MAIN SCREEN
  [1-9]     open a channel
  [d]       direct messages
  [p]       your profile
  [n]       create a new channel
  [q]       quit

INSIDE A CHANNEL
  type      send a message
  [a]       send an anonymous message
  [f]       send a file (paste the full file path)
  [b]       go back

DIRECT MESSAGES
  type      send a message
  [f]       send a file
  [b]       go back

GUEST MODE (unregistered_user)
  - Can read and post in channels
  - Cannot DM, create channels, or edit profile
  - Username is always "unregistered_user"
```

---

## SHARING WITH FRIENDS

Once you have the `.exe` built:
- Send your friends the `underground.exe` file
- They run it and register or join as guest
- Everyone connects to the same Railway server

> No installation needed on their end — just the .exe file.

---

## NOTES

- Files up to 10MB can be shared
- Anonymous posts show as "anonymous" — sender is still logged server-side
- Messages persist in the database
- Railway free plan sleeps after inactivity — first connection may take ~5 seconds to wake up
- To upgrade storage/uptime, Railway paid plan is $5/month

---

## FOLDER STRUCTURE

```
underground/
├── server/
│   ├── index.js          ← the backend (Node.js)
│   ├── package.json
│   └── railway.toml      ← Railway deploy config
└── client/
    ├── underground.py    ← the terminal app (Python)
    ├── requirements.txt
    └── build.bat         ← run this to build the .exe
```
