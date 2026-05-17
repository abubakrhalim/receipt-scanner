# Receipt Scanner — AI Auto-Fill Web App

Upload a receipt image and let Claude AI extract the key fields instantly, then review and submit the pre-filled form.

## Live Demo

> Deploy to Vercel (see below) or open `index.html` directly in any browser — no server needed.

## Features

- Drag-and-drop or click-to-upload receipt images (JPG, PNG, WEBP, GIF)
- Claude Vision API extracts **merchant name**, **date**, **total amount**, and **currency**
- Editable form pre-filled with extracted data
- Submit saves entries in-memory for the session
- Single HTML file — zero dependencies, zero build step

## How to Run Locally

1. Clone or download this repo
2. Open `index.html` in your browser (double-click or `File → Open`)
3. Enter your [Claude API key](https://console.anthropic.com/) in the key field
4. Upload a receipt image and click **Extract Receipt Data**

No Node.js, no npm, no build required.

## Deploy to Vercel

```bash
# Install Vercel CLI (requires Node.js)
npm i -g vercel

# Deploy from the receipt-app directory
vercel
```

Or connect the GitHub repo in the [Vercel dashboard](https://vercel.com) — it will auto-detect the static site.

## Model & Prompt

**Model:** `gemini-2.0-flash` — Google's free vision model (1,500 requests/day free, no credit card required)

**Get a free API key:** https://aistudio.google.com — click "Get API key", takes ~30 seconds.

**Prompt strategy:**
```
You are a receipt data extraction assistant. Carefully examine this receipt image
and extract the following four fields. Respond ONLY with a valid JSON object —
no markdown, no explanation, nothing else.

Required JSON format:
{
  "merchant_name": "<name of the store or business, or null if not found>",
  "date": "<date in YYYY-MM-DD format, or null if not found>",
  "total_amount": "<numeric total as a string e.g. '12.50', or null if not found>",
  "currency": "<3-letter ISO 4217 currency code e.g. USD, EUR, GBP, or null if not found>"
}
```

The prompt instructs the model to return **pure JSON only** (no markdown fences), which makes parsing robust and avoids hallucinated prose. Fields the model cannot find are returned as `null` and the form shows an empty input.

## Tech Stack

| Layer | Choice |
|---|---|
| Frontend | Vanilla HTML / CSS / JS (single file) |
| AI | Google Gemini API — `gemini-2.0-flash` (free tier) |
| Hosting | Static — Vercel / any CDN / local file |
| Storage | In-memory (`submissions[]` array) |

## API Key Security Note

The API key is typed into the browser and sent directly to `generativelanguage.googleapis.com`. It is **never logged or stored** by this app. For production use, proxy the request through a backend to keep the key server-side.
