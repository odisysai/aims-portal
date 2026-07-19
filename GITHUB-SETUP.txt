AIMS Portal – GitHub Repository Setup Guide
============================================
Generated: 19 Jul 2026 | v1.0
Follow these steps after unzipping the HTML files into your repo.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
STEP 1 — REPO FOLDER STRUCTURE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Create this folder structure in your GitHub repo:

  aims-portal/
  ├── README.md                        ← paste README.txt contents
  ├── CONTRIBUTING.md                  ← paste CONTRIBUTING section below
  ├── .gitignore                       ← paste GITIGNORE section below
  ├── package.json                     ← paste PACKAGE.JSON section below
  ├── .github/
  │   ├── workflows/
  │   │   ├── deploy.yml               ← paste DEPLOY.YML below
  │   │   ├── validate.yml             ← paste VALIDATE.YML below
  │   │   └── preview.yml              ← paste PREVIEW.YML below
  │   └── ISSUE_TEMPLATE/
  │       ├── bug_report.md            ← paste BUG REPORT below
  │       └── design_change.md         ← paste DESIGN CHANGE below
  ├── .devcontainer/
  │   └── devcontainer.json            ← paste DEVCONTAINER.JSON below
  ├── .vscode/
  │   ├── extensions.json              ← paste VSCODE EXTENSIONS below
  │   └── settings.json                ← paste VSCODE SETTINGS below
  ├── apps/
  │   ├── homepage/
  │   │   └── index.html               ← AIMS-Homepage.html
  │   ├── agentic-ui/
  │   │   └── index.html               ← AIMS-Agentic-UI-Workspaces.html
  │   ├── inspector/
  │   │   └── index.html               ← AIMS-Inspector-App.html
  │   ├── blueprint/
  │   │   └── index.html               ← AIMS-Architecture-Blueprint-Interactive.html
  │   └── developer-portal/
  │       └── index.html               ← AIMS-Developer-Portal.html
  └── docs/
      └── AIMS-Architecture-Blueprint.html  ← AIMS-Blueprint-Doc.html


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
STEP 2 — ENABLE GITHUB PAGES
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
1. Go to your repo → Settings → Pages
2. Source: GitHub Actions (not branch deploy)
3. Push to main → deploy.yml triggers automatically
4. Portal live at: https://your-org.github.io/aims-portal/

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
STEP 3 — ENABLE PR PREVIEW (optional, Netlify)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
1. Create a free Netlify account at netlify.com
2. Add secrets to your GitHub repo:
   - NETLIFY_AUTH_TOKEN  (from Netlify → User Settings → Applications)
   - NETLIFY_SITE_ID     (from Netlify site → Site Settings → General)
3. Every PR gets a unique preview URL posted as a comment automatically


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
FILE: .gitignore
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
node_modules/
.DS_Store
Thumbs.db
*.log
npm-debug.log*
.env
.env.local
.netlify/
dist/
.cache/


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
FILE: package.json
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
{
  "name": "aims-portal",
  "version": "1.0.0",
  "description": "AIMS Asset Integrity Management System – Design & Architecture Portal",
  "private": true,
  "scripts": {
    "dev": "live-server --port=3000 --open=apps/developer-portal/index.html",
    "validate": "html-validate apps/*/index.html docs/*.html",
    "a11y": "node scripts/a11y-check.js",
    "format": "prettier --write 'apps/**/*.html' 'docs/**/*.html'",
    "build": "echo 'No build step needed — static HTML files are production-ready'"
  },
  "devDependencies": {
    "html-validate": "^9.0.0",
    "live-server": "^1.2.2",
    "prettier": "^3.3.0",
    "axe-core": "^4.9.0",
    "playwright": "^1.44.0"
  },
  "html-validate": {
    "extends": ["html-validate:recommended"],
    "rules": {
      "no-inline-style": "off",
      "require-sri": "off",
      "script-type": "off",
      "void-style": "off"
    }
  },
  "engines": { "node": ">=20.0.0" },
  "license": "UNLICENSED"
}


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
FILE: .github/workflows/deploy.yml
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
name: Deploy to GitHub Pages

on:
  push:
    branches: [main]
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: pages
  cancel-in-progress: false

jobs:
  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/configure-pages@v4
      - name: Create root redirect to Dev Portal
        run: cp apps/developer-portal/index.html index.html
      - uses: actions/upload-pages-artifact@v3
        with:
          path: '.'
      - id: deployment
        uses: actions/deploy-pages@v4
      - name: Post deployment summary
        run: |
          echo "## Deployed" >> $GITHUB_STEP_SUMMARY
          echo "Portal: ${{ steps.deployment.outputs.page_url }}" >> $GITHUB_STEP_SUMMARY


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
FILE: .github/workflows/validate.yml
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
name: Validate HTML

on:
  pull_request:
    branches: [main, develop]

jobs:
  validate:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: 'npm'
      - run: npm ci
      - run: npm run validate
      - name: File size report
        run: |
          echo "| File | Size |" >> $GITHUB_STEP_SUMMARY
          echo "|------|------|" >> $GITHUB_STEP_SUMMARY
          for f in apps/*/index.html docs/*.html; do
            echo "| $f | $(du -sh $f | cut -f1) |" >> $GITHUB_STEP_SUMMARY
          done


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
FILE: .github/workflows/preview.yml
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
name: PR Preview Deploy

on:
  pull_request:
    types: [opened, synchronize, reopened]

jobs:
  preview:
    runs-on: ubuntu-latest
    permissions:
      pull-requests: write
      contents: read
    steps:
      - uses: actions/checkout@v4
      - id: netlify
        uses: nwtgck/actions-netlify@v3
        with:
          publish-dir: '.'
          production-deploy: false
          github-token: ${{ secrets.GITHUB_TOKEN }}
          deploy-message: "PR Preview — ${{ github.event.pull_request.title }}"
          enable-pull-request-comment: true
          alias: aims-portal-pr-${{ github.event.pull_request.number }}
        env:
          NETLIFY_AUTH_TOKEN: ${{ secrets.NETLIFY_AUTH_TOKEN }}
          NETLIFY_SITE_ID: ${{ secrets.NETLIFY_SITE_ID }}


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
FILE: .devcontainer/devcontainer.json
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
{
  "name": "AIMS Portal Dev Environment",
  "image": "mcr.microsoft.com/devcontainers/javascript-node:20",
  "features": {
    "ghcr.io/devcontainers/features/github-cli:1": {}
  },
  "customizations": {
    "vscode": {
      "extensions": [
        "ritwickdey.LiveServer",
        "esbenp.prettier-vscode",
        "GitHub.copilot",
        "GitHub.copilot-chat",
        "ms-vscode.live-server",
        "bradlc.vscode-tailwindcss",
        "formulahendry.auto-rename-tag",
        "eamodio.gitlens"
      ],
      "settings": {
        "liveServer.settings.port": 3000,
        "liveServer.settings.root": "/",
        "editor.formatOnSave": true,
        "editor.tabSize": 2
      }
    }
  },
  "postCreateCommand": "npm install",
  "forwardPorts": [3000],
  "portsAttributes": {
    "3000": {
      "label": "AIMS Portal (Live Server)",
      "onAutoForward": "openBrowser"
    }
  }
}


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
FILE: .vscode/extensions.json
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
{
  "recommendations": [
    "ritwickdey.LiveServer",
    "esbenp.prettier-vscode",
    "GitHub.copilot",
    "GitHub.copilot-chat",
    "bradlc.vscode-tailwindcss",
    "formulahendry.auto-rename-tag",
    "eamodio.gitlens"
  ]
}


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
FILE: CONTRIBUTING.md
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
See full CONTRIBUTING guide in README.txt.

Quick PR checklist:
- [ ] HTML validates (npm run validate passes)
- [ ] Tested in Chrome and Firefox
- [ ] No console errors in browser DevTools
- [ ] Design tokens match AIMS system (colors in README)
- [ ] Added review board comment if architectural decision changed

Useful Copilot prompts for editing:
  @workspace Update the Planner intent catalog in apps/blueprint/index.html
  @workspace Add a new agent card to apps/agentic-ui/index.html
  @workspace Sync the color palette across all apps in apps/

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
DESIGN TOKENS (quick reference for edits)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
bg-deep:    #070B12   Page background
bg-surface: #0C1521   Panel background
bg-card:    #101D30   Card background
border:     #1C2E47   Subtle borders
primary:    #0A84FF   Primary/Planner
teal:       #00C7BE   Reviewer/evidence
amber:      #FF9F0A   Supervisor/warnings
purple:     #BF5AF2   Inspector/Gemma
green:      #32D74B   Success/verified
red:        #FF3B30   Errors/critical
text-pri:   #F5F5F7   Primary text
text-sec:   #8E8E93   Secondary text
