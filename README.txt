# AIMS – Asset Integrity Management System · Design & Architecture Portal

> Industrial periodic inspections · Edge intelligence · Multi-agent workflows · Governed evidence chains

[![Deploy to GitHub Pages](https://github.com/your-org/aims-portal/actions/workflows/deploy.yml/badge.svg)](https://github.com/your-org/aims-portal/actions/workflows/deploy.yml)
[![Validate HTML](https://github.com/your-org/aims-portal/actions/workflows/validate.yml/badge.svg)](https://github.com/your-org/aims-portal/actions/workflows/validate.yml)
[![Open in GitHub Codespaces](https://github.com/codespaces/badge.svg)](https://codespaces.new/your-org/aims-portal)

---

## 📦 What's in This Repository

This repo contains all AIMS interactive design and architecture artefacts. Every file is a self-contained single-page HTML app — no build step required.

| App | Path | Description |
|-----|------|-------------|
| 🌐 **Homepage** | `apps/homepage/index.html` | Campaign landing page — hero, lifecycle, competitive table, customer outcomes |
| 💼 **Agentic UI** | `apps/agentic-ui/index.html` | Multi-role workspace shell — Planner, Reviewer, Supervisor with AG-UI chat |
| 📱 **Inspector App** | `apps/inspector/index.html` | Field mobile/tablet PWA — Gemma-4-2B-IT offline agent, capture, escalation |
| 📐 **Blueprint** | `apps/blueprint/index.html` | Interactive architecture blueprint — intent catalog, contracts, auth matrix |
| 🏛️ **Dev Portal** | `apps/developer-portal/index.html` | Team reference hub — all deliverables, review board, decisions log |
| 📄 **Blueprint Doc** | `docs/AIMS-Architecture-Blueprint.html` | Printable architecture reference document |

---

## 🚀 Quick Start

### Option 1 — Open directly in browser
Each HTML file is fully self-contained. Just double-click any `index.html` file to open it locally — no server needed.

### Option 2 — Live preview with VS Code
```bash
# Clone the repo
git clone https://github.com/your-org/aims-portal.git
cd aims-portal

# Install dependencies (Live Server + html-validate)
npm install

# Start dev server with hot reload on port 3000
npm run dev
```
Then open `http://localhost:3000` — changes to any HTML file reload automatically.

### Option 3 — GitHub Codespaces (zero-install, browser-based editing)
Click the **"Open in GitHub Codespaces"** badge above. Your full dev environment launches in the browser with Live Server pre-configured. No local setup needed.

---

## 🌍 Deployed URLs (GitHub Pages)

After merging to `main`, GitHub Actions deploys all apps automatically:

| App | URL |
|-----|-----|
| Dev Portal (hub) | `https://your-org.github.io/aims-portal/` |
| Homepage | `https://your-org.github.io/aims-portal/apps/homepage/` |
| Agentic UI | `https://your-org.github.io/aims-portal/apps/agentic-ui/` |
| Inspector App | `https://your-org.github.io/aims-portal/apps/inspector/` |
| Blueprint | `https://your-org.github.io/aims-portal/apps/blueprint/` |

---

## 📁 Repository Structure

```
aims-portal/
├── .devcontainer/
│   └── devcontainer.json        # GitHub Codespaces config
├── .github/
│   ├── workflows/
│   │   ├── deploy.yml           # Auto-deploy to GitHub Pages on push to main
│   │   ├── validate.yml         # HTML validation on every PR
│   │   └── preview.yml          # PR preview deployment (Netlify)
│   └── ISSUE_TEMPLATE/
│       ├── bug_report.md
│       └── design_change.md
├── .vscode/
│   ├── extensions.json          # Recommended extensions
│   └── settings.json           # Editor settings + live server config
├── apps/
│   ├── homepage/index.html
│   ├── agentic-ui/index.html
│   ├── inspector/index.html
│   ├── blueprint/index.html
│   └── developer-portal/index.html
├── docs/
│   └── AIMS-Architecture-Blueprint.html
├── .gitignore
├── CONTRIBUTING.md
├── package.json
└── README.md
```

---

## ✏️ How to Edit

### For developers (VS Code + Copilot)
1. Open any `apps/*/index.html` file in VS Code
2. GitHub Copilot has full `@workspace` context — ask it to modify any section
3. Live Server hot-reloads your changes instantly
4. Open a PR → GitHub Actions validates HTML and deploys a preview URL

### For architects (GitHub Codespaces)
1. Click "Open in Codespaces" — no local install needed
2. Edit directly in the browser-based VS Code
3. Preview at the Codespaces forwarded port URL
4. Commit and PR from within Codespaces

### Recommended VS Code Copilot prompts
```
@workspace Update the Planner intent catalog in apps/blueprint/index.html to add a new intent called 'generate_risk_report'
@workspace Add a new agent card to the Supervisor workspace in apps/agentic-ui/index.html for an EvidenceAuditAgent
@workspace Sync the color palette in apps/inspector/index.html to match the updated design tokens
```

---

## 🔄 Automated Workflows

| Workflow | Trigger | What it does |
|----------|---------|--------------|
| `deploy.yml` | Push to `main` | Deploys all apps to GitHub Pages |
| `validate.yml` | Every PR | Runs html-validate on all HTML files, reports errors inline |
| `preview.yml` | Every PR | Deploys ephemeral Netlify preview, posts URL to PR comment |

---

## 🤝 Contributing

See [CONTRIBUTING.md](./CONTRIBUTING.md) for the full contribution guide, including:
- How to propose design changes
- PR checklist for HTML apps
- How to add a new app to the portal
- Review board process

---

## 📋 Architecture Quick Reference

| Layer | Technology | Status |
|-------|-----------|--------|
| Field Edge | Inspector PWA · Gemma-4-2B-IT (INT4) | Designed |
| Transport | AG-UI · SSE · UUIDv7 threadId | Designed |
| Workspace | A2UI · Planner/Reviewer/Supervisor | Designed |
| Orchestration | 6 cloud agents | Designed |
| Governance | SHA-256 evidence chain | Designed |
| Persistence | Redis (hot) · PostgreSQL (cold) | Planned |

**Core principle:** The agent may propose the UI and action. Authorization and execution remain in deterministic backend services.

---

## 🏷️ Version History

| Version | Date | Changes |
|---------|------|---------|
| v1.0 | 19 Jul 2026 | Initial release — all 5 apps + architecture doc |

---

*AIMS Engineering Portal · Internal Use · © 2026 AIMS Technologies Inc.*
