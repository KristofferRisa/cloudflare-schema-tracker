# Cloudflare GraphQL Schema Tracker

This repository automatically tracks changes to the **Cloudflare GraphQL API schema**. It specifically monitors the **Viewer → Zones → HttpRequests1dGroups** structure.

## 🚀 How It Works

1. A **GitHub Actions workflow** runs **daily at 03:42 UTC**.
2. It fetches the **relevant schema parts** from Cloudflare’s GraphQL API.
3. If any changes are detected, it commits and pushes them to the repository.

## 📂 Repository Structure

/cloudflare-schema-tracker
│── .github/workflows/scrape-cloudflare-schema.yml # GitHub Actions workflow
│── cloudflare/
│ ├── cloudflare-schema-scoped.json # Tracked schema file
│── .gitignore # Ignore unnecessary files
│── README.md # Documentation
│── LICENSE # Open-source license (MIT)

## 🛠️ Setup Instructions

### 1️⃣ Fork and Clone

```sh
git clone https://github.com/YOUR_USERNAME/cloudflare-schema-tracker.git
cd cloudflare-schema-tracker
```

### 2️⃣ Add GitHub Secrets

Go to GitHub → Settings → Secrets and Variables → Actions, and add:
• CLOUDFLARE_EMAIL → Your Cloudflare account email.
• CLOUDFLARE_AUTH_KEY → Your Cloudflare Global API Key.

### 3️⃣ Manually Run the Workflow

Trigger a manual run from GitHub Actions → Scrape Cloudflare GraphQL Schema.

### 🔍 How to Test Locally

```sh
Run this command (replace placeholders):
curl -X POST "https://api.cloudflare.com/client/v4/graphql" \
  -H "X-Auth-Email: your-email@example.com" \
  -H "X-Auth-Key: your-global-api-key" \
  -H "Content-Type: application/json" \
  -d '{"query": "query IntrospectionQuery { __schema { types(filter: { name_in: [\"Viewer\", \"Zone\", \"HttpRequests1dGroup\"] }) { name kind description fields { name type { name kind ofType { name kind } } } } } }"}' \
  | jq '.'
```
