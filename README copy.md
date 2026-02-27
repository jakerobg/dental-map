# Medicaid Dental Utilization Map

Interactive choropleth map of Medicaid dental procedure utilization across Massachusetts ZIP Code Tabulation Areas. Built with MapLibre GL JS, PMTiles, and Vite + React.

## Quick Start

```bash
npm install
cp .env.example .env.local
# Add your Protomaps API key to .env.local (free at protomaps.com/dashboard)
npm run dev
```

## Deploy to GitHub Pages

**1. Set your repo name in `vite.config.ts`:**
```ts
base: '/your-repo-name/'
```

**2. Add your Protomaps key as a GitHub repository secret:**
- Go to repo Settings → Secrets → Actions
- Add secret: `VITE_PROTOMAPS_API_KEY`

**3. Deploy:**
```bash
npm run build
npm run deploy
```

Or set up GitHub Actions to deploy automatically on push (see below).

## GitHub Actions (auto-deploy on push)

Create `.github/workflows/deploy.yml`:

```yaml
name: Deploy to GitHub Pages
on:
  push:
    branches: [main]
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: 20
      - run: npm install
      - run: npm run build
        env:
          VITE_PROTOMAPS_API_KEY: ${{ secrets.VITE_PROTOMAPS_API_KEY }}
      - uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./dist
```

## Swapping in Real Data

Replace `public/data/ma-dental.json` with real aggregated data from your PostGIS pipeline. The format is:

```json
{
  "01001": {
    "all": 87,
    "preventive": 42,
    "restorative": 28,
    "extractions": 12,
    "ortho": 5,
    "providers": 4,
    "beneficiaries": 1200,
    "totalPayment": 94000,
    "avgPaymentPerClaim": 720
  }
}
```

## Tech Stack

| | |
|---|---|
| Map renderer | MapLibre GL JS 4.x |
| Basemap | Protomaps (white style) |
| ZCTA geometry | Census TIGER → Tippecanoe → PMTiles |
| Attribute data | Static JSON (swap for API when ready) |
| Frontend | React 18 + Vite |
| State | Zustand |
