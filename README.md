# SunThru Facility Inventory

Mobile-friendly Flask app for looking up and updating parts inventory stored in Google Sheets.

---

## Local Setup

### 1. Prerequisites
- Python 3.11+
- `credentials.json` service account key file (see Google setup below)

### 2. Install dependencies

```bash
cd "/Users/leza/Documents/SunThru/Inventory project/claude code"
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

### 3. Add credentials

Place `credentials.json` in the project root.  
**Never commit this file** — it is already in `.gitignore`.

### 4. Run locally

```bash
flask run
```

Open `http://localhost:5000` to search, or go directly to:

```
http://localhost:5000/item?id=PART_NAME_HERE
```

---

## Google Sheets Setup

1. Go to [Google Cloud Console](https://console.cloud.google.com/).
2. Enable **Google Sheets API** and **Google Drive API**.
3. Create a **Service Account**, download the JSON key, rename it `credentials.json`.
4. Share the Google Sheet with the service account email as **Editor**:
   `sunthru-inventory@sunthru-inventory.iam.gserviceaccount.com`

---

## Deploy to Render

### 1. Push to GitHub

```bash
git init
git add .
git commit -m "initial commit"
git remote add origin https://github.com/YOUR_ORG/sunthru-inventory.git
git push -u origin main
```

`credentials.json` and `.env` are gitignored and will not be pushed.

### 2. Create a Web Service on Render

1. [render.com](https://render.com) → **New → Web Service**
2. Connect your GitHub repo
3. Set:
   - **Build Command:** `pip install -r requirements.txt`
   - **Start Command:** `gunicorn app:app`

### 3. Add environment variables in Render

| Key | Value |
|-----|-------|
| `SHEET_ID` | `1rQDVBLL6aBYw-Jhv8J-s8OHi4zNBqF5Lc2o_j-bhbuA` |
| `CREDENTIALS_PATH` | `/etc/secrets/credentials.json` |

### 4. Upload credentials.json as a Secret File

Render dashboard → **Secret Files** → add file named `/etc/secrets/credentials.json` → paste the full JSON content of your service account key.

---

## Sheet Structure

| Tab | Header row | Data starts |
|-----|-----------|-------------|
| F1000 | Row 1 | Row 2 |
| E1000 | Row 1 | Row 2 |
| W1000 | Row 2 | Row 3 |
| L1000 | Row 2 | Row 3 |

Columns: Part Name, Location Code, Qty, Condition, Reorder Threshold, Reorder Flag, Photo/Link, QR Code.
