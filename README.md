# SG Cycling Paths — Interactive Map

Interactive map showing Singapore's cycling path network, sourced from data.gov.sg.

## Files in this repo

- **`index.html`** — the map page
- **`cycling_paths.geojson`** — the cycling path data (loaded by the page)

## How to refresh the data

The dataset rarely changes, so it's stored as a file rather than fetched live. To update it:

### Step 1 — Download fresh data from data.gov.sg

Easiest way (no coding needed):

1. Go to the dataset page: <https://data.gov.sg/datasets/d_8f468b25193f64be8a16fa7d8f60f553/view>
2. Click the **Download** button → save the file
3. Rename it to `cycling_paths.geojson`

Or if you prefer the API (Python):

```python
import requests, json

dataset_id = "d_8f468b25193f64be8a16fa7d8f60f553"
poll = requests.get(
    f"https://api-open.data.gov.sg/v1/public/api/datasets/{dataset_id}/poll-download"
).json()

if poll["code"] != 0:
    raise Exception(poll["errMsg"])

data = requests.get(poll["data"]["url"]).json()

with open("cycling_paths.geojson", "w") as f:
    json.dump(data, f)
```

### Step 2 — Upload to GitHub

1. In your repo, click **Add file → Upload files**
2. Drag in the new `cycling_paths.geojson` (replaces the old one)
3. Click **Commit changes**

### Step 3 — See the update

GitHub Pages refreshes within ~1 minute. Open your site and click the **↻** button on the map (or hard-refresh the page) — the banner should show the new "Updated" date.

## How it works

The HTML file does just one fetch — to `cycling_paths.geojson` in the same folder. No API calls, no CORS proxy, no external dependencies for data. Loads instantly.

If the data file is missing or corrupt, the map falls back to a small embedded sample (12 paths) and shows an amber warning banner.
