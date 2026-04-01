# Educational Review — Deployment Guide

## Overview

- **Domain**: educationalreview.org (currently on Squarespace)
- **Hosting**: Cloudflare Pages (free)
- **Source**: GitHub repository (free)
- **Cost**: $0/month for hosting. Domain renewal only (~$10-15/year).

## Site Structure

```
educationalreview.org/
├── index.html                          ← Landing page (lists all publications)
├── enduring-university/
│   ├── index.html                      ← The Enduring University essay site
│   └── tree-metaphor.png              ← Tree diagram image
├── future-publication/                 ← (future) Next publication
│   ├── index.html
│   └── (any images or assets)
├── DEPLOY-GUIDE.md                     ← This file
└── README.md                           ← (optional) GitHub repo readme
```

Each publication lives in its own folder with an `index.html`.
This gives clean URLs: `educationalreview.org/enduring-university`

---

## Step-by-Step Setup (One-Time)

### 1. Create a GitHub Repository

1. Go to https://github.com/new
2. Name it `educationalreview.org` (or any name you like)
3. Set it to **Public** (required for free Cloudflare Pages)
4. Don't add README yet — we'll push our files
5. Click "Create repository"

### 2. Push the Site Files to GitHub

On your Mac, open Terminal and run:

```bash
# Navigate to the site folder
cd /path/to/educationalreview.org

# Initialize git
git init
git add .
git commit -m "Initial site with Enduring University essay"

# Connect to your GitHub repo (replace YOUR_USERNAME)
git remote add origin https://github.com/YOUR_USERNAME/educationalreview.org.git
git branch -M main
git push -u origin main
```

### 3. Create a Cloudflare Account (Free)

1. Go to https://dash.cloudflare.com/sign-up
2. Sign up with your email
3. You do NOT need to add your domain to Cloudflare DNS yet (that's Step 5)

### 4. Set Up Cloudflare Pages

1. In the Cloudflare dashboard, go to **Workers & Pages** → **Create**
2. Click the **Pages** tab
3. Click **Connect to Git**
4. Authorize Cloudflare to access your GitHub
5. Select the `educationalreview.org` repository
6. Configure the build:
   - **Project name**: `educationalreview` (or whatever you prefer)
   - **Production branch**: `main`
   - **Build command**: (leave empty — it's a static site, no build needed)
   - **Build output directory**: `/` (root — our files are already HTML)
7. Click **Save and Deploy**

Your site will be live at `educationalreview.pages.dev` within about 30 seconds.
Test it works before proceeding.

### 5. Connect Your Domain

There are two options. Option A is simpler. Option B is cheaper long-term.

#### Option A: Keep Domain at Squarespace, Point to Cloudflare Pages

1. In Cloudflare Pages → your project → **Custom domains** → **Set up a custom domain**
2. Enter `educationalreview.org` and click **Continue**
3. Cloudflare will give you a CNAME record to add
4. Go to Squarespace → **Domains** → **educationalreview.org** → **DNS Settings**
5. Add the CNAME record Cloudflare gave you (something like `educationalreview.pages.dev`)
6. Also add `www.educationalreview.org` as a CNAME pointing to the same place
7. Wait for DNS to propagate (usually 5-30 minutes, can take up to 48 hours)

#### Option B: Transfer Domain to Cloudflare (Recommended — Cheapest Long-Term)

Cloudflare Registrar charges at-cost pricing (~$10/year for .org).
Squarespace charges more.

1. In Cloudflare dashboard → **Domain Registration** → **Transfer**
2. Enter `educationalreview.org`
3. Follow the steps — you'll need to:
   - Unlock the domain in Squarespace
   - Get the transfer authorization code from Squarespace
   - Confirm the transfer
4. Transfer takes 5-7 days
5. Once transferred, go back to Cloudflare Pages → Custom domains → add `educationalreview.org`
6. Cloudflare will auto-configure DNS since it owns both the domain and hosting

### 6. Verify

- Visit `https://educationalreview.org` — should show the landing page
- Visit `https://educationalreview.org/enduring-university` — should show the essay
- SSL should be automatic (Cloudflare handles this)

---

## Adding a New Publication (Future Workflow)

This is the process you'll follow every time:

### 1. Build the site

Work with Claude (or manually) to create the HTML site for the new piece.

### 2. Add it to the repo

```bash
# Create a new folder
mkdir new-publication-slug

# Put the index.html and any images inside
cp /path/to/new-site/* new-publication-slug/
```

### 3. Update the landing page

Open `index.html` (the root landing page) and add a new publication card.
Copy the template comment in the HTML and fill in the details.

### 4. Push to GitHub

```bash
git add .
git commit -m "Add new publication: Title Here"
git push
```

Cloudflare Pages auto-deploys. Your new publication is live within ~30 seconds
at `educationalreview.org/new-publication-slug`.

---

## LLM and SEO Friendliness

Each publication page includes:

- **Schema.org structured data** (`ScholarlyArticle` type) in JSON-LD — this is what Google, Bing, and LLM crawlers (ChatGPT, Perplexity, Claude) use to understand the content
- **Open Graph meta tags** — for rich previews when shared on social media
- **Canonical URLs** — prevents duplicate content issues
- **Semantic HTML** — proper heading hierarchy, article structure
- **Full essay text inline** — LLM crawlers can read the entire essay directly from the HTML (no JavaScript rendering required)

The landing page includes `WebSite` schema markup.

---

## Troubleshooting

**Site not updating after push?**
Check Cloudflare Pages → your project → **Deployments** to see if the build succeeded.

**Domain not working?**
DNS propagation can take up to 48 hours. Check with: https://dnschecker.org

**HTTPS not working?**
Cloudflare auto-provisions SSL. If it's not working, go to Cloudflare → SSL/TLS and make sure it's set to "Full" or "Flexible".

**Want to preview changes before going live?**
Cloudflare Pages creates a preview URL for every push to a non-main branch. Push to a branch called `draft` and you'll get a preview URL like `abc123.educationalreview.pages.dev`.

---

## Summary of Costs

| Item | Cost |
|------|------|
| Cloudflare Pages hosting | Free (unlimited bandwidth) |
| GitHub repository | Free |
| SSL certificate | Free (Cloudflare) |
| Domain (if transferred to Cloudflare) | ~$10-12/year |
| Domain (if kept at Squarespace) | ~$20/year |
| **Total** | **$10-20/year** |
