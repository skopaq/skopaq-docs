# Architecture Overview

Welcome to the Skopaq architecture documentation. This section provides in-depth technical details about how Skopaq is designed and built.

## Core Components

Skopaq is built on a modern, cloud-native architecture that combines:

- **LangGraph Orchestration** - Multi-agent workflow management with durable execution
- **Multi-Model AI** - Intelligent routing across 45+ models from 9 providers
- **Real-time Streaming** - Server-Sent Events for live test execution updates
- **Self-Healing Intelligence** - AI-powered test maintenance and repair

## Documentation Sections

| Section | Description |
|---------|-------------|
| [Skopaq Brain](brain.md) | Deep dive into the AI code intelligence system |
| [Architecture Diagrams](diagrams.md) | Visual representations of system components |

## Quick Architecture Overview

```
┌─────────────────────────────────────────────────────────────────┐
│                    ARGUS PLATFORM                                │
├─────────────────────────────────────────────────────────────────┤
│  Dashboard (Next.js)  │  API (FastAPI)  │  Worker (Cloudflare)  │
├─────────────────────────────────────────────────────────────────┤
│                   LangGraph Orchestrator                         │
│  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐           │
│  │  Code    │ │   Test   │ │   Self   │ │ Reporter │           │
│  │ Analyzer │ │ Executor │ │  Healer  │ │  Agent   │           │
│  └──────────┘ └──────────┘ └──────────┘ └──────────┘           │
├─────────────────────────────────────────────────────────────────┤
│  Supabase (PostgreSQL + pgvector)  │  Browser Pool (Selenium)   │
└─────────────────────────────────────────────────────────────────┘
```

## Key Design Principles

1. **AI-First** - Every feature leverages AI for intelligent automation
2. **Privacy-Preserving** - Your code never leaves your infrastructure
3. **Cost-Optimized** - Intelligent model routing minimizes API costs
4. **Self-Healing** - Tests automatically adapt to UI changes

## Next Steps

- Explore the [Skopaq Brain Architecture](brain.md) for AI capabilities
- Review [Architecture Diagrams](diagrams.md) for visual reference
- Check the [API Reference](../api/index.md) for integration details
