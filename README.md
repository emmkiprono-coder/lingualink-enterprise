# LinguaLink Enterprise

**Advocate Health Language Services Command Center**

A 12-module enterprise platform for managing language services operations across 6 states (IL, WI, NC, SC, GA, AL), 160+ interpreters, and $40M+ annual operations. Includes AI-powered agents for autonomous research, compliance analysis, and strategic planning.

---

## Quick Start

```bash
# Clone
git clone https://github.com/YOUR_ORG/lingualink-enterprise.git
cd lingualink-enterprise

# Install
npm install

# Run locally
npm run dev

# Build for production
npm run build
```

Open [http://localhost:3000](http://localhost:3000)

---

## Modules (12 Tabs)

| # | Module | Purpose |
|---|--------|---------|
| 1 | **Interpreter Match** | Filter 18+ interpreters by language, location, modality. Language network visualization with mutual intelligibility, bridge languages, relay chains. |
| 2 | **Translation Engine** | Clinical phrase translation with register awareness, confidence scoring, AI translation output with mandatory human verification warnings. |
| 3 | **Demand Heatmap** | 500 simulated live encounters with language distribution, modality breakdown, state filtering, gap analysis. |
| 4 | **Vendor Cost Simulator** | 4-vendor comparison (Language Line, AMN, GLOBO, Stratus) with current vs prior rates, 2026 projections, savings calculator. |
| 5 | **Relay Protocol** | 8-step interpreter relay checklist with critical flags, escalation triggers, authorization logic for complex language chains. |
| 6 | **Dialect Clusters** | 3 high-risk clusters (Arabic, Chin-Karen, South Asian) with intelligibility matrices, risk scenarios, dispatch guidance. |
| 7 | **Language ID** | 20 languages with native scripts, greetings, tap-to-match workflow for frontline staff identification. |
| 8 | **Translation QA** | 3 translation tiers, 5 scoring dimensions (accuracy, fluency, terminology, formatting, completeness). |
| 9 | **Real Impact** | 21 documented malpractice cases with deaths, lawsuits, settlements. Full source citations with clickable links. Severity filtering. |
| 10 | **Research** | 8 peer-reviewed studies with full citations, DOI links, methodology, expandable drilldowns. |
| 11 | **Demographics** | 6-state LEP forecasting with 2024-2030 projections, refugee data, language trend analysis. |
| 12 | **AI Agents** | 3 autonomous AI agents powered by Claude API with persistent storage. |

---

## AI Agents

The platform includes 3 AI-powered agents that use the Anthropic Claude API with web search:

### Case Research Agent
Searches the web for real documented cases of language barrier failures in healthcare. Returns structured case data with severity classification, source URLs, and Advocate Health relevance analysis. Save cases to your persistent library.

### Compliance Advisor
Answers regulatory questions about Title VI, ACA Section 1557, Joint Commission standards, CMS requirements, and state-specific regulations. Returns risk-level analysis, applicable regulations, prioritized recommendations, and state notes for all 6 Advocate Health states.

### Strategic Planner
Generates operational plans with phased implementation, KPIs (current vs target), ROI projections, risk assessments, and owner assignments. Pre-loaded with Advocate Health organizational context.

### Persistent Storage
Saved cases and insights persist across browser sessions using the storage API. Data is scoped to the current user.

---

## Data Sources

### Documented Cases (21)
All cases include full source citations with clickable links to primary sources:
- National Health Law Program (NHLP) Malpractice Report
- AMA Journal of Ethics
- NPR Health Shots Investigation
- ACLU of New Mexico filings
- NBC Los Angeles
- PBG Law, Medical Justice
- Boostlingo, K-International compilations

### Research Studies (8)
Peer-reviewed studies from JGIM, Annals of Emergency Medicine, Health Affairs, Joint Commission Journal, Medical Care, JAMA Internal Medicine.

---

## Deploy to Vercel

### Option A: One-Click (Recommended)

[![Deploy with Vercel](https://vercel.com/button)](https://vercel.com/new/clone?repository-url=https://github.com/YOUR_ORG/lingualink-enterprise)

### Option B: Vercel CLI

```bash
# Install Vercel CLI
npm install -g vercel

# Login
vercel login

# Deploy (will auto-detect Vite)
vercel

# Deploy to production
vercel --prod
```

### Option C: GitHub Actions (CI/CD)

The repo includes a GitHub Actions workflow at `.github/workflows/deploy.yml` that auto-deploys:
- **Push to `main`** → Production deployment
- **Pull requests** → Preview deployment with URL comment

#### Required GitHub Secrets

Go to repo Settings > Secrets and variables > Actions, and add:

| Secret | How to Get |
|--------|-----------|
| `VERCEL_TOKEN` | [vercel.com/account/tokens](https://vercel.com/account/tokens) → Create Token |
| `VERCEL_ORG_ID` | Run `vercel` locally, check `.vercel/project.json` → `orgId` |
| `VERCEL_PROJECT_ID` | Run `vercel` locally, check `.vercel/project.json` → `projectId` |

---

## GitHub Setup

### Initial Push

```bash
# Initialize git
git init
git add .
git commit -m "Initial commit: LinguaLink Enterprise v1.0"

# Create repo on GitHub (using GitHub CLI)
gh repo create lingualink-enterprise --private --source=. --push

# Or manually add remote and push
git remote add origin https://github.com/YOUR_ORG/lingualink-enterprise.git
git branch -M main
git push -u origin main
```

### Branch Strategy

```
main          ← Production (auto-deploys)
├── develop   ← Integration branch
│   ├── feature/new-module
│   ├── feature/agent-enhancement
│   └── fix/field-mapping
```

---

## Environment Variables

For the AI Agents to work in production, the Anthropic API key is handled client-side through the artifact sandbox. If deploying as a standalone app outside Claude.ai, you'll need to add a proxy:

```env
# .env.local (never commit this)
VITE_ANTHROPIC_API_KEY=sk-ant-...
```

Then update the `apiCall` function in `App.jsx` to use the env var or a backend proxy.

---

## Architecture

```
lingualink-enterprise/
├── .github/
│   └── workflows/
│       └── deploy.yml          # CI/CD: lint, build, deploy
├── public/
│   └── favicon.svg             # LL monogram
├── src/
│   ├── App.jsx                 # Main app (12 modules + 3 agents)
│   ├── main.jsx                # React entry point
│   └── index.css               # Global styles + animations
├── .gitignore
├── index.html                  # Vite entry HTML
├── package.json                # Dependencies + scripts
├── vercel.json                 # Vercel deployment config
├── vite.config.js              # Vite build config
└── README.md
```

### Data Constants (in App.jsx)

| Constant | Size | Description |
|----------|------|-------------|
| `LR` | 5.6KB | Language relationships, mutual intelligibility, bridges, relay chains |
| `CASES` | 27.7KB | 21 documented malpractice cases with full citations + URLs |
| `ORES` | 4.9KB | 8 peer-reviewed research studies |
| `INTERPS` | 1.9KB | 18 interpreters with languages, modalities, locations |
| `DEMOP` | 1.4KB | 6-state demographic projections |
| `DC` | 1.6KB | 3 dialect cluster matrices |
| `LID` | 1.7KB | 20 languages with native scripts |
| Others | ~5KB | STRATS, VR, VOLS, RELAY, RESC, FACS, etc. |

---

## Tech Stack

- **React 18** — UI framework
- **Vite 5** — Build tool (dev server + production bundler)
- **Anthropic Claude API** — AI agents (Sonnet 4 with web search)
- **Persistent Storage API** — Cross-session data persistence
- **Vercel** — Hosting + edge network
- **GitHub Actions** — CI/CD pipeline

---

## License

Proprietary — Advocate Health internal use only.

---

*Built for Emmanuel Chepkwony, AVP Enterprise Language Services & Access, Advocate Health*
