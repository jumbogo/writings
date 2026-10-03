# Deployment

The site is built and published to GitHub Pages directly from this repository
by `.github/workflows/deploy.yml`. No deploy keys or second repository are needed.

## One-time setup

In this repository: **Settings** → **Pages**:
- **Build and deployment → Source**: select **GitHub Actions**
- **Custom domain**: `jumbochow.com`, then tick **Enforce HTTPS** once the certificate is issued

DNS for `jumbochow.com` is at Namecheap: four `A` records on `@` to
185.199.108.153, 185.199.109.153, 185.199.110.153, 185.199.111.153, and a
`CNAME` from `www` to `jumbogo.github.io`.

## How it works

On every push to `main` (or a manual run from the **Actions** tab) the workflow:

1. Checks out the repo with the theme submodule
2. Builds with a pinned Hugo version (`hugo --minify --gc`)
3. Publishes `jumbo-space/public` to GitHub Pages

The base URL comes from GitHub Pages at build time, so renaming the repository
or adding a custom domain needs no config change.

## Upgrading Hugo

Bump `hugo-version` in the workflow after checking the site builds locally with
the new version:

```bash
cd jumbo-space && hugo --gc
```

## Local preview

```bash
cd jumbo-space && hugo server
```

Build output (`public/`, `resources/_gen/`) is git-ignored.
