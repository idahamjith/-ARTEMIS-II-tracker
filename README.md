# Artemis II — Mission Control Dashboard

Real-time mission dashboard for NASA's Artemis II crewed lunar flyby.  
Live telemetry pulled from **JPL Horizons** every 5 minutes via GitHub Actions.

---

## How it works

```
GitHub Actions (every 5 min)
  → fetches JPL Horizons API (server-side, no CORS issues)
  → parses X/Y/Z position + velocity vectors for Artemis II (ID: -1024)
  → commits data.json to this repo

GitHub Pages
  → serves index.html + data.json as a static site
  → dashboard reads data.json every 5 min
  → extrapolates position every second using real physics
```

---

## Deploy in 4 steps

### 1. Fork or create this repo on GitHub
Upload all files keeping the folder structure:
```
your-repo/
├── index.html
├── data.json
└── .github/
    └── workflows/
        └── fetch-horizons.yml
```

### 2. Enable GitHub Pages
- Go to **Settings → Pages**
- Source: **Deploy from a branch**
- Branch: `main` / folder: `/ (root)`
- Click **Save**

### 3. Enable GitHub Actions write permission
- Go to **Settings → Actions → General**
- Scroll to **Workflow permissions**
- Select **Read and write permissions**
- Click **Save**

This is required so the Action can commit `data.json` back to the repo.

### 4. Trigger the first run manually
- Go to **Actions → Fetch Horizons Telemetry**
- Click **Run workflow**

After ~30 seconds `data.json` will be populated with real data.  
From then on it runs automatically every 5 minutes.

---

## Your live URL
```
https://YOUR-USERNAME.github.io/YOUR-REPO-NAME/
```

---

## Telemetry fields in data.json

| Field | Description |
|---|---|
| `rg` | Distance from Earth center (km) |
| `rr` | Range-rate — how fast distance is changing (km/s) |
| `x/y/z` | ECI position vector (km) |
| `vx/vy/vz` | ECI velocity vector (km/s) |
| `speed` | Scalar speed — magnitude of velocity vector (km/s) |
| `moonDist` | Distance from Moon center (km) |
| `fetchedAt` | ISO timestamp of last Horizons fetch |

The dashboard interpolates position every second using `pos = pos₀ + vel × Δt`  
so numbers update live without hammering the API.

---

## Spacecraft ID
Artemis II (Orion "Integrity") = **-1024** in JPL Horizons
