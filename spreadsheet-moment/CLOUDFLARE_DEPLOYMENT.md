# Cloudflare Deployment Instructions

## Repository Status

✅ **Repository:** https://github.com/SuperInstance/spreadsheet-moment
✅ **Status:** Live on GitHub
✅ **Commit:** 5968984 - Initial release complete

## What You Need to Do on Cloudflare

### Step 1: Access Your Cloudflare Dashboard

1. Go to https://dash.cloudflare.com
2. Log in with your Cloudflare account

### Step 2: Set Up Cloudflare Pages (for the Website)

1. Navigate to **Workers & Pages**
2. Click **Create Application**
3. Select **Pages**
4. Connect to **GitHub**
5. Select **SuperInstance/spreadsheet-moment** repository
6. Build settings:
   - **Build command:** `cd website && npm install && npm run build`
   - **Build output directory:** `website/dist`
   - **Root directory:** `/`
7. Click **Save and Deploy**

### Step 3: Configure Custom Domain

1. In Pages settings, click **Custom Domains**
2. Add: `spreadsheet-moment.superinstance.ai`
3. Click **Activate Domain**
4. Update DNS at superinstance.ai:
   - Type: **CNAME**
   - Name: `spreadsheet-moment`
   - Target: Cloudflare provides this

### Step 4: Set Up Workers (for API)

1. Navigate to **Workers & Pages**
2. Click **Create Application**
3. Select **Create Worker**
4. Name: `spreadsheet-moment-api`
5. Upload code from `cloudflare/worker.js`
6. Configure **wrangler.toml** settings:
   - KV Namespaces
   - Durable Objects
   - R2 Buckets
7. Deploy

### Step 5: Configure Environment Variables

In Workers > Settings > Environment Variables:

```
ENVIRONMENT = production
API_URL = https://api.superinstance.ai
SECRET_KEY = [generate a strong secret]
```

### Step 6: Set Up KV Namespaces

1. Navigate to **Workers & Pages** > **KV**
2. Create namespace: `cell_state`
3. Create namespace: `cell_state_preview`
4. Note the IDs for wrangler.toml

### Step 7: Set Up R2 Bucket (for Spreadsheets)

1. Navigate to **R2**
2. Create bucket: `spreadsheet-moment-sheets`
3. Enable public access (if needed)

### Step 8: Configure Routes

For **superinstance.ai/spreadsheet-moment**:

1. Go to **superinstance.ai** zone in Cloudflare
2. Navigate to **Workers Routes**
3. Add route:
   - Route: `superinstance.ai/spreadsheet-moment/*`
   - Worker: `spreadsheet-moment-api`
   - Service: `spreadsheet-moment-api`

## Deploy Commands

### Initial Deployment

```bash
cd C:\Users\casey\polln\spreadsheet-moment
npm install -g wrangler
wrangler login
wrangler pages deploy website/dist --project-name=spreadsheet-moment
```

### Update Deployment

```bash
cd website
npm run build
cd ..
wrangler pages deploy website/dist --project-name=spreadsheet-moment
```

## What's Deployed

### Website
- URL: https://spreadsheet-moment.superinstance.ai (once DNS propagates)
- Pages: Home, Features, Documentation, Examples, Download
- Tech: React + Vite

### API (Worker)
- URL: https://superinstance.ai/spreadsheet-moment/api/*
- Endpoints: /api/cells, /api/agent, etc.
- Tech: Cloudflare Worker with KV, Durable Objects, R2

## Next Steps (After You Complete Cloudflare Setup)

1. **Test the website** at the deployed URL
2. **Test API endpoints** with curl or Postman
3. **Set up monitoring** in Cloudflare dashboard
4. **Configure analytics** (Google Analytics, etc.)
5. **Set up error tracking** (Sentry, etc.)

## Need Help?

Cloudflare Documentation:
- Pages: https://developers.cloudflare.com/pages
- Workers: https://developers.cloudflare.com/workers
- KV: https://developers.cloudflare.com/kv
- R2: https://developers.cloudflare.com/r2

## Status

✅ GitHub repository live
⏳ Awaiting Cloudflare configuration
⏳ Awaiting DNS propagation

Once you complete the Cloudflare steps, the site will be live at:
**https://superinstance.ai/spreadsheet-moment**
