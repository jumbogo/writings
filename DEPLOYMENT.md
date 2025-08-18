# Automated Deployment Setup

This repository is configured for automated deployment to GitHub Pages using GitHub Actions.

## Setup Instructions

### 1. Generate SSH Deploy Key

```bash
ssh-keygen -t rsa -b 4096 -C "github-actions-deploy" -f deploy_key -N ""
```

This creates two files:
- `deploy_key` (private key)
- `deploy_key.pub` (public key)

### 2. Configure Deploy Key in Target Repository

1. Go to your **target repository**: `https://github.com/jumboalex/jumboalex.github.io`
2. Navigate to: **Settings** → **Deploy keys** → **Add deploy key**
3. Add the **public key** (`deploy_key.pub` content):
   - **Title**: `GitHub Actions Deploy Key`
   - **Key**: Paste the content of `deploy_key.pub`
   - ✅ **Allow write access** (important!)
4. Click **Add key**

### 3. Add Secret to Source Repository

1. Go to your **source repository**: `https://github.com/jumboalex/writings`
2. Navigate to: **Settings** → **Secrets and variables** → **Actions**
3. Click **New repository secret**
4. Add the **private key**:
   - **Name**: `ACTIONS_DEPLOY_KEY`
   - **Secret**: Paste the content of `deploy_key` (the private key file)
5. Click **Add secret**

### 4. Clean Up

After adding the keys to GitHub, delete the local key files for security:

```bash
rm deploy_key deploy_key.pub
```

## How It Works

The workflow (`.github/workflows/deploy.yml`) will:

1. **Trigger** on every push to the `main` branch
2. **Build** the Hugo site with minification and optimization
3. **Deploy** the generated files to `jumboalex.github.io` repository
4. **Update** GitHub Pages automatically

## Manual Trigger

You can also trigger deployment manually:
1. Go to **Actions** tab in your repository
2. Select **Deploy Hugo Site to GitHub Pages**
3. Click **Run workflow**

## Build Optimizations Added

- **Minification**: CSS, HTML, JS, JSON, SVG, XML
- **Cache configuration**: Faster builds with intelligent caching
- **Garbage collection**: Cleans up unused files
- **Production environment**: Optimized for performance

## Monitoring Deployments

- View deployment status in the **Actions** tab
- Check deployment history in your GitHub Pages repository
- Site updates should be live within 2-3 minutes after push

## Troubleshooting

**Common issues:**
- Ensure deploy key has **write access** enabled
- Verify the secret name is exactly `ACTIONS_DEPLOY_KEY`
- Check that the target repository name is correct in the workflow
- Make sure Hugo theme submodule is properly initialized