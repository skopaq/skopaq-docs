# Quick Start Guide

## 1-Minute Setup

```bash
# Install
pip install -e .

# Set API key
export ANTHROPIC_API_KEY=sk-ant-...

# Run (your app must be running!)
e2e-agent --codebase ./your-app --app-url http://localhost:3000
```

## What Happens

```
Your Code + Running App
         │
         ▼
   ┌───────────────┐
   │ 1. ANALYZE    │  Claude reads your code
   │    CODE       │  Finds pages, APIs, models
   └───────┬───────┘
           ▼
   ┌───────────────┐
   │ 2. PLAN       │  Creates test plan
   │    TESTS      │  Prioritizes critical paths
   └───────┬───────┘
           ▼
   ┌───────────────┐
   │ 3. RUN        │  Opens browser
   │    TESTS      │  Clicks, types, validates
   └───────┬───────┘
           ▼
   ┌───────────────┐
   │ 4. FIX        │  If test fails
   │    FAILURES   │  AI fixes & retries
   └───────┬───────┘
           ▼
   ┌───────────────┐
   │ 5. REPORT     │  Results + screenshots
   └───────────────┘
```

## Common Commands

```bash
# Full test run
e2e-agent -c ./app -u http://localhost:3000

# PR-focused testing (faster, cheaper)
e2e-agent -c ./app -u http://localhost:3000 --pr 123 --changed-files src/login.tsx

# Save results to specific folder
e2e-agent -c ./app -u http://localhost:3000 -o ./my-results
```

## Output

```
./test-results/
├── results.json     # Machine-readable results
├── report.html      # Human-readable report
└── screenshots/     # Failure screenshots
```

## Configuration (.env file)

```bash
ANTHROPIC_API_KEY=sk-ant-...    # Required
COST_LIMIT_PER_RUN=10.00        # Optional: max cost
SELF_HEAL_ENABLED=true          # Optional: auto-fix tests
```

## Next Steps

- 📖 [Full Workflows Guide](./WORKFLOWS.md)
- 🧩 [Chrome Extension Setup](../extension/README.md)
- ⚙️ [Configuration Options](workflows.md#configuration)
