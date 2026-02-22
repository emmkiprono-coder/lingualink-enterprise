# LinguaLink Enterprise — Deployment Guide
## Step-by-Step: GitHub + Vercel

---

## STEP 1: Create GitHub Repository

### Option A: GitHub Web UI
1. Go to https://github.com/new
2. Repository name: `lingualink-enterprise`
3. Visibility: **Private**
4. Do NOT initialize with README (we already have one)
5. Click **Create repository**

### Option B: GitHub CLI
```bash
gh repo create lingualink-enterprise --private
```

---

## STEP 2: Push Code to GitHub

```bash
cd lingualink-enterprise

# Initialize git
git init

# Add all files
git add .

# First commit
git commit -m "feat: LinguaLink Enterprise v1.0 - 12 modules + 3 AI agents"

# Set main branch
git branch -M main

# Add your remote (replace YOUR_USERNAME)
git remote add origin https://github.com/YOUR_USERNAME/lingualink-enterprise.git

# Push
git push -u origin main
```

---

## STEP 3: Connect to Vercel

### Option A: Vercel Dashboard (Easiest)
1. Go to https://vercel.com/new
2. Click **Import Git Repository**
3. Select `lingualink-enterprise`
4. Framework Preset: **Vite** (auto-detected)
5. Build Command: `npm run build` (auto-detected)
6. Output Directory: `dist` (auto-detected)
7. Click **Deploy**
8. Wait ~60 seconds
9. Your site is live at `https://lingualink-enterprise.vercel.app`

### Option B: Vercel CLI
```bash
# Install
npm install -g vercel

# Login
vercel login

# Deploy (follow prompts)
vercel

# First time: it will ask:
#   Set up and deploy? Y
#   Which scope? (select your account)
#   Link to existing project? N
#   Project name? lingualink-enterprise
#   Directory? ./
#   Override settings? N

# Deploy to production
vercel --prod
```

---

## STEP 4: Set Up CI/CD (Optional but Recommended)

This enables auto-deployment on every push to `main` and preview URLs for pull requests.

### 4a. Get Vercel Credentials

After Step 3, run locally:
```bash
cat .vercel/project.json
```
You'll see:
```json
{
  "orgId": "team_xxxxxxxxxx",
  "projectId": "prj_xxxxxxxxxx"
}
```

Also create a Vercel token:
1. Go to https://vercel.com/account/tokens
2. Click **Create**
3. Name: `github-actions`
4. Scope: Full Account
5. Copy the token

### 4b. Add GitHub Secrets

1. Go to your repo on GitHub
2. Settings > Secrets and variables > Actions
3. Click **New repository secret** for each:

| Name | Value |
|------|-------|
| `VERCEL_TOKEN` | The token from step 4a |
| `VERCEL_ORG_ID` | The `orgId` from `.vercel/project.json` |
| `VERCEL_PROJECT_ID` | The `projectId` from `.vercel/project.json` |

### 4c. Verify

Push a change:
```bash
git add .
git commit -m "chore: test CI/CD pipeline"
git push
```

Go to GitHub repo > Actions tab to watch the deployment.

---

## STEP 5: Custom Domain (Optional)

1. In Vercel Dashboard, go to your project
2. Settings > Domains
3. Add your domain: `lingualink.advocatehealth.com`
4. Vercel will provide DNS records
5. Add the records in your DNS provider:
   - Type: `CNAME`
   - Name: `lingualink`
   - Value: `cname.vercel-dns.com`
6. Wait for DNS propagation (usually <5 min)

---

## Updating the App

### Quick update (direct to production):
```bash
# Make changes to src/App.jsx
git add .
git commit -m "feat: add new module"
git push  # auto-deploys via CI/CD
```

### Safe update (with preview):
```bash
# Create a branch
git checkout -b feature/new-module

# Make changes
git add .
git commit -m "feat: add new module"
git push -u origin feature/new-module

# Open a PR on GitHub
# CI/CD will deploy a preview URL
# Review the preview
# Merge PR to main
# CI/CD auto-deploys to production
```

---

## Troubleshooting

### Build fails
```bash
# Test locally first
npm run build
# Check for errors in terminal output
```

### Vercel deployment fails
- Check Vercel Dashboard > Deployments > click failed deployment > Logs
- Most common: missing dependency or build error

### AI Agents not working
- Agents use the Anthropic API which requires authentication
- Inside Claude.ai artifacts: works automatically
- Standalone deployment: requires an API proxy (see README for setup)

### Preview URL not appearing on PR
- Check that all 3 GitHub secrets are set correctly
- Check Actions tab for error logs

---

## File Manifest

```
lingualink-enterprise/
├── .github/workflows/deploy.yml    ← CI/CD pipeline
├── .gitignore                      ← Git exclusions
├── README.md                       ← Project documentation
├── DEPLOY.md                       ← This file
├── index.html                      ← Vite entry HTML
├── package.json                    ← Node dependencies
├── vercel.json                     ← Vercel config + headers
├── vite.config.js                  ← Vite build settings
├── public/
│   └── favicon.svg                 ← LL monogram icon
└── src/
    ├── App.jsx                     ← Main application (97KB)
    ├── main.jsx                    ← React mount point
    └── index.css                   ← Global styles
```
