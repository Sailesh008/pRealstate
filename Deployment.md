

> Complete guide for hosting setup, environment configuration, CI/CD pipeline, and going live.

---

## Table of Contents

- [Overview](#overview)
- [Prerequisites](#prerequisites)
- [Hosting Setup](#hosting-setup)
- [Environment Variables](#environment-variables)
- [CI/CD Pipeline](#cicd-pipeline)
- [Deployment Steps](#deployment-steps)
- [Live URL](#live-url)
- [Rollback Procedure](#rollback-procedure)
- [Troubleshooting](#troubleshooting)

---

## Overview

| Item              | Details                          |
|-------------------|----------------------------------|
| **Platform**      | (e.g. Vercel / AWS / Railway)    |
| **Runtime**       | (e.g. Node.js 20 / Python 3.12)  |
| **Branch → Env**  | `main` → Production, `dev` → Staging |
| **Live URL**      | https://your-app.example.com     |
| **Deployed by**   | GitHub Actions (automated)       |

---

## Prerequisites

Before deploying, make sure you have:

- [ ] Access to the hosting platform account
- [ ] GitHub repository secrets configured (see [Environment Variables](#environment-variables))
- [ ] Domain name purchased and DNS access (for custom domains)
- [ ] Database provisioned (if applicable)
- [ ] Required third-party API keys obtained

---

## Hosting Setup

### 1. Create Hosting Account

Sign up or log in to your hosting provider (e.g. [Vercel](https://vercel.com), [Render](https://render.com), [Railway](https://railway.app), or AWS).

### 2. Connect the Repository

```bash
# Example: Vercel CLI
npm install -g vercel
vercel login
vercel link   # Link to existing project or create new
```

Or connect via the provider's dashboard:
1. Navigate to **New Project → Import Git Repository**
2. Select your GitHub repository
3. Set the **root directory**, **build command**, and **output directory**

### 3. Configure Build Settings

| Setting            | Value                        |
|--------------------|------------------------------|
| **Build Command**  | `npm run build`              |
| **Output Dir**     | `dist` / `.next` / `build`   |
| **Install Command**| `npm install`                |
| **Node Version**   | `20.x`                       |

### 4. Custom Domain (Optional)

1. Go to **Project Settings → Domains**
2. Add your domain: `yourdomain.com`
3. Update your DNS provider with the records shown:

```
Type    Name    Value
A       @       76.76.21.21
CNAME   www     cname.vercel-dns.com
```

> ⏱ DNS propagation can take up to 48 hours.

---

## Environment Variables

### Required Variables

| Variable              | Description                          | Example                          |
|-----------------------|--------------------------------------|----------------------------------|
| `NODE_ENV`            | Runtime environment                  | `production`                     |
| `DATABASE_URL`        | Full database connection string      | `postgresql://user:pass@host/db` |
| `SECRET_KEY`          | App secret / JWT signing key         | `a-long-random-string`           |
| `API_BASE_URL`        | Base URL of the backend API          | `https://api.yourdomain.com`     |
| `NEXT_PUBLIC_APP_URL` | Public-facing app URL (frontend)     | `https://yourdomain.com`         |

> **Never commit secrets to the repository.** Always use your platform's secret manager or GitHub Actions secrets.

### Setting Variables in GitHub Actions

Go to **Repository → Settings → Secrets and variables → Actions → New repository secret** and add each variable listed above.

### Setting Variables on Hosting Platform

```bash
# Vercel CLI example
vercel env add DATABASE_URL production
vercel env add SECRET_KEY production

# Or set via dashboard:
# Project → Settings → Environment Variables
```

### Local Development `.env`

Create a `.env.local` file (already in `.gitignore`):

```env
NODE_ENV=development
DATABASE_URL=postgresql://localhost:5432/myapp_dev
SECRET_KEY=dev-secret-not-for-production
API_BASE_URL=http://localhost:3001
NEXT_PUBLIC_APP_URL=http://localhost:3000
```

---

## CI/CD Pipeline

The pipeline is powered by **GitHub Actions** and runs automatically on every push.

### Workflow File

Save this as `.github/workflows/deploy.yml`:

```yaml
name: CI / CD Pipeline

on:
  push:
    branches:
      - main        # Triggers production deploy
      - dev         # Triggers staging deploy
  pull_request:
    branches:
      - main

jobs:
  # ── 1. Test ────────────────────────────────────────────────
  test:
    name: Run Tests
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Set up Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: 'npm'

      - name: Install dependencies
        run: npm ci

      - name: Run linter
        run: npm run lint

      - name: Run unit tests
        run: npm test -- --coverage

  # ── 2. Build ───────────────────────────────────────────────
  build:
    name: Build Application
    runs-on: ubuntu-latest
    needs: test
    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Set up Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: 'npm'

      - name: Install dependencies
        run: npm ci

      - name: Build
        run: npm run build
        env:
          NODE_ENV: production

      - name: Upload build artifact
        uses: actions/upload-artifact@v4
        with:
          name: build-output
          path: dist/

  # ── 3. Deploy to Staging ───────────────────────────────────
  deploy-staging:
    name: Deploy to Staging
    runs-on: ubuntu-latest
    needs: build
    if: github.ref == 'refs/heads/dev'
    environment: staging
    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Deploy to Staging
        uses: amondnet/vercel-action@v25
        with:
          vercel-token: ${{ secrets.VERCEL_TOKEN }}
          vercel-org-id: ${{ secrets.VERCEL_ORG_ID }}
          vercel-project-id: ${{ secrets.VERCEL_PROJECT_ID }}
          alias-domains: staging.yourdomain.com

  # ── 4. Deploy to Production ────────────────────────────────
  deploy-production:
    name: Deploy to Production
    runs-on: ubuntu-latest
    needs: build
    if: github.ref == 'refs/heads/main'
    environment: production
    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Deploy to Production
        uses: amondnet/vercel-action@v25
        with:
          vercel-token: ${{ secrets.VERCEL_TOKEN }}
          vercel-org-id: ${{ secrets.VERCEL_ORG_ID }}
          vercel-project-id: ${{ secrets.VERCEL_PROJECT_ID }}
          vercel-args: '--prod'

      - name: Notify Slack on Success
        if: success()
        uses: slackapi/slack-github-action@v1
        with:
          payload: '{"text":"✅ Production deployed successfully!"}'
        env:
          SLACK_WEBHOOK_URL: ${{ secrets.SLACK_WEBHOOK_URL }}
```

### Pipeline Overview

```
Push to `main` or `dev`
        │
        ▼
  ┌─────────────┐
  │  Run Tests  │  ← lint + unit tests
  └──────┬──────┘
         │ pass
         ▼
  ┌─────────────┐
  │    Build    │  ← npm run build
  └──────┬──────┘
         │
    ┌────┴────┐
    │         │
    ▼         ▼
Staging    Production
 (dev)      (main)
```

---

## Deployment Steps

### First-Time Deploy

```bash
# 1. Clone the repository
git clone https://github.com/your-org/your-repo.git
cd your-repo

# 2. Install dependencies
npm install

# 3. Set up environment variables
cp .env.example .env.local
# Edit .env.local with real values

# 4. Run database migrations (if applicable)
npm run db:migrate

# 5. Build the application
npm run build

# 6. Deploy manually (first time)
vercel --prod
```

### Subsequent Deploys (Automated)

```bash
# Just push to main — GitHub Actions handles the rest
git add .
git commit -m "feat: your feature description"
git push origin main
```

### Manual Deploy (Emergency)

```bash
# Build and deploy directly from local machine
npm run build
vercel --prod --token=$VERCEL_TOKEN
```

---

## Live URL

| Environment    | URL                                      | Branch  |
|----------------|------------------------------------------|---------|
| **Production** | https://your-app.example.com             | `main`  |
| **Staging**    | https://staging.your-app.example.com     | `dev`   |
| **Local**      | http://localhost:3000                    | any     |

---

## Rollback Procedure

### Via Hosting Dashboard

1. Go to **Project → Deployments**
2. Find the last stable deployment
3. Click **⋯ → Promote to Production**

### Via CLI

```bash
# List recent deployments
vercel ls

# Rollback to a specific deployment URL
vercel rollback https://your-app-abc123.vercel.app
```

### Via Git

```bash
# Revert the last commit and push
git revert HEAD
git push origin main
# GitHub Actions will automatically redeploy the reverted version
```

---

## Troubleshooting

### Build Fails

```bash
# Check build logs in GitHub Actions or run locally:
npm run build 2>&1 | tee build.log

# Common fixes:
# - Missing env variables → add them to GitHub secrets
# - Dependency errors → delete node_modules and run npm ci
# - Type errors → run npm run lint to identify issues
```

### Deployment Times Out

- Check hosting platform status page
- Verify your `build` command completes in under the provider's time limit
- Consider splitting large builds or enabling build caching

### Environment Variables Not Loaded

- Confirm they are set in **both** the hosting platform and GitHub secrets
- For frontend variables, ensure they are prefixed with `NEXT_PUBLIC_` (Next.js) or `VITE_` (Vite)
- Redeploy after adding new variables — they are not hot-reloaded

### Database Connection Errors

```bash
# Verify DATABASE_URL is correct
echo $DATABASE_URL

# Test connection
npx prisma db pull   # or your ORM equivalent

# Check firewall / IP allowlist on your database provider
```

---

## Additional Resources

- [Platform Docs — Vercel](https://vercel.com/docs)
- [GitHub Actions Docs](https://docs.github.com/en/actions)
- [12-Factor App: Config](https://12factor.net/config)
- [OWASP Secrets Management](https://cheatsheetseries.owasp.org/cheatsheets/Secrets_Management_Cheat_Sheet.html)

---

*Last updated: May 2026 · Maintained by the platform team*



