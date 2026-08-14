# Deploying APAW to Vercel

## What's in this folder
```
apaw_vercel/
├── api/
│   └── index.py       ← the Flask app (Vercel auto-detects Python here)
├── vercel.json         ← routes every request to api/index.py
├── requirements.txt    ← flask
└── DEPLOY.md            ← this file
```

## Option A — fastest (Vercel CLI, no GitHub needed)
1. Install the CLI once: `npm install -g vercel`
2. From inside this `apaw_vercel` folder, run:
   ```
   vercel
   ```
3. Answer the prompts (link/create a project, defaults are fine).
4. It builds and gives you a live URL like `https://apaw-xyz.vercel.app` — that's your dashboard.
5. Every time you re-run `vercel --prod`, it redeploys with your latest changes.

## Option B — GitHub-connected (auto-redeploys on push)
1. Push this folder to a GitHub repo.
2. Go to vercel.com → **Add New Project** → import that repo.
3. Vercel detects the Python function automatically — no build settings needed.
4. Deploy. You get a permanent URL, and future `git push`es auto-redeploy.

## URLs once deployed
- `https://your-project.vercel.app/` — Command Center (what you present)
- `https://your-project.vercel.app/resident` — Resident portal (share this as a QR code for the panel to try uploading a photo live)
- `https://your-project.vercel.app/controls` — Presenter tools (open on your own phone/laptop, not the projector)

## The one thing to know before you demo
This build **doesn't use a background thread** to update sensors — Vercel's serverless functions don't support that. Instead, each request generates fresh readings on the spot, which looks identical on screen but is what makes it deployable here at all.

The report queue and simulation mode are still simple in-memory variables. That's fine for a live demo (Vercel keeps the function warm across your session's requests), but don't count on it holding data for days — a cold start can reset it. That's expected and doesn't need explaining to the panel; it's just not the Pi build's persistence model. When you're back on the actual Raspberry Pi hardware, use `apaw_app.py` (the other file) — that one keeps the real background sensor loop and is the actual field-deployment target.