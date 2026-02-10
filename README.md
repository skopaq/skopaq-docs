# ARGUS Documentation

Private documentation site for ARGUS - AI-Powered Autonomous E2E Testing Platform.

## Live Site

**URL**: https://docs.skopaq.ai (Private - Cloudflare Access protected)

## Quick Start

### Local Development

```bash
# Install dependencies
pip install mkdocs-material mkdocs-minify-plugin

# Serve locally
mkdocs serve

# Build static site
mkdocs build
```

### Deploy to Cloudflare Pages

```bash
# Using Wrangler
./scripts/deploy-docs.sh

# Or manually
npx wrangler pages deploy site --project-name=argus-docs
```

## Documentation Structure

```
docs-site/
├── index.md                    # Homepage
├── getting-started/            # Getting Started Guide
├── product/                    # Product Documentation
│   ├── executive-summary.md
│   ├── pitch-deck.md
│   └── knowledge-base.md
├── deployment/                 # Deployment Options
│   ├── index.md
│   └── faq.md
├── sales/                      # Sales Materials
│   ├── battlecard.md
│   └── competitive-analysis.md
└── technical/                  # Technical Documentation
    ├── architecture.md
    ├── implementation.md
    └── security.md
```

## Cloudflare Setup

See [DOCS_SETUP_INSTRUCTIONS.md](./DOCS_SETUP_INSTRUCTIONS.md) for complete setup guide including:

- Cloudflare Pages deployment
- Custom domain (docs.skopaq.ai)
- Cloudflare Access for private authentication
- GitHub Actions auto-deployment

## Configuration Files

| File | Purpose |
|------|---------|
| `mkdocs.yml` | MkDocs configuration |
| `wrangler.toml` | Cloudflare Pages config |
| `.github/workflows/deploy-docs.yml` | Auto-deploy on push |

## Security

Documentation is protected by Cloudflare Access (Zero Trust):
- Email-based authentication
- Only authorized users can access
- All traffic encrypted via Cloudflare SSL
- Audit logs in Zero Trust dashboard

---

*ARGUS - AI-Powered Autonomous E2E Testing Platform*
