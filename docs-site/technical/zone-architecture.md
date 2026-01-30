# Argus Quality Intelligence Platform: 3-Zone Architecture

> **PhD-Level Systems Architecture Analysis**
> Comprehensive feature isolation between Dashboard, MCP, and shared infrastructure

---

## Executive Summary

The Argus platform operates across **three distinct zones** with carefully designed boundaries:

| Zone | Access Mode | Primary Users | Data Flow |
|------|-------------|---------------|-----------|
| **Zone 1: Dashboard-Only** | Web browser (Clerk auth) | QA Engineers, Managers | Visualize, Configure, Integrate |
| **Zone 2: MCP-Accessible** | AI Agent (API key/OAuth2) | Claude Code, Cursor, VS Code | Query intelligence, Execute tests |
| **Zone 3: Shared/Real-time** | Both interfaces | All users | Sync state, stream events |

---

## Zone 1: Dashboard-Only Features

Features that are **exclusively cloud-based** and accessible only through the web dashboard.

### 1.1 User & Organization Management

| Feature | Endpoint | Why Dashboard-Only |
|---------|----------|-------------------|
| User signup/login | `/api/auth/*` | Clerk OAuth flow requires browser |
| Organization creation | `/api/organizations` | Billing, compliance setup |
| Team management | `/api/teams/*` | Role assignment, invitations |
| Billing & subscription | `/api/billing/*` | Stripe integration, invoices |
| Audit logs | `/api/audit-logs` | Compliance, SOC2 reporting |

### 1.2 Integration Configuration (OAuth Flows)

These require browser-based OAuth2 consent flows:

| Integration | OAuth Scopes | Dashboard-Only Reason |
|-------------|--------------|----------------------|
| **GitHub** | `repo`, `read:org`, `admin:repo_hook` | Webhook registration |
| **GitLab** | `api`, `read_repository` | Pipeline triggers |
| **Jira** | `read:jira-work`, `write:jira-work` | Issue linking |
| **Confluence** | `read:confluence-content.all` | Knowledge indexing |
| **Sentry** | `project:read`, `event:read` | Error correlation |
| **Slack** | `chat:write`, `channels:read` | Notifications |
| **Linear** | `read`, `write` | Sprint sync |
| **PagerDuty** | `read`, `write` | Incident triggers |

```
┌─────────────────────────────────────────────────────────────────┐
│                    DASHBOARD (Zone 1)                            │
│                                                                  │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐           │
│  │   GitHub     │  │    Jira      │  │   Sentry     │           │
│  │  OAuth Flow  │  │  OAuth Flow  │  │  OAuth Flow  │           │
│  └──────┬───────┘  └──────┬───────┘  └──────┬───────┘           │
│         │                  │                  │                   │
│         └──────────────────┼──────────────────┘                  │
│                            ▼                                      │
│                 ┌──────────────────┐                             │
│                 │  Integration     │                             │
│                 │  Token Store     │                             │
│                 │  (Supabase)      │                             │
│                 └──────────────────┘                             │
└─────────────────────────────────────────────────────────────────┘
```

### 1.3 Visual Analytics & Reporting

| Feature | Dashboard Page | API Endpoint |
|---------|----------------|--------------|
| Test execution timeline | `/dashboard/analytics` | `/api/analytics/timeline` |
| Failure heatmaps | `/dashboard/analytics/heatmap` | `/api/analytics/heatmap` |
| Coverage visualization | `/dashboard/coverage` | `/api/coverage/map` |
| Flaky test rankings | `/dashboard/quality/flaky` | `/api/quality/flaky-ranking` |
| Cost optimization | `/dashboard/cost` | `/api/cost/breakdown` |
| Export reports (PDF) | `/dashboard/reports/export` | `/api/reports/generate` |

### 1.4 Configuration Management

| Setting | Dashboard Section | MCP Access |
|---------|-------------------|------------|
| Project settings | `/dashboard/settings` | Read-only via API |
| Notification rules | `/dashboard/notifications` | ❌ None |
| Webhook endpoints | `/dashboard/webhooks` | ❌ None |
| API key management | `/dashboard/api-keys` | ❌ None (security) |
| Custom test templates | `/dashboard/templates` | Read-only |
| Environment variables | `/dashboard/environments` | ❌ None (secrets) |

### 1.5 Collaborative Features

| Feature | Why Dashboard-Only |
|---------|-------------------|
| Test review comments | Real-time collaboration UI |
| Approval workflows | Human-in-the-loop decisions |
| Shared workspaces | Multi-user coordination |
| Session replay viewer | Video streaming, timeline scrubbing |
| Screenshot comparison slider | Visual diff UI |

---

## Zone 2: MCP-Accessible Features

Features accessible through the Argus MCP server for AI agents (Claude Code, Cursor, etc.).

### 2.1 MCP Tool Categories (73 Tools)

```
┌─────────────────────────────────────────────────────────────────┐
│                      MCP SERVER (Zone 2)                         │
│                                                                  │
│  ┌─────────────────────────────────────────────────────────┐    │
│  │                  73 GATEWAY TOOLS                        │    │
│  │                                                          │    │
│  │  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐   │    │
│  │  │ Discovery│ │Execution │ │Healing   │ │Analytics │   │    │
│  │  │ 12 tools │ │ 8 tools  │ │ 9 tools  │ │ 6 tools  │   │    │
│  │  └──────────┘ └──────────┘ └──────────┘ └──────────┘   │    │
│  │                                                          │    │
│  │  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐   │    │
│  │  │ Coverage │ │ Reports  │ │Browser   │ │ CI/CD    │   │    │
│  │  │ 4 tools  │ │ 3 tools  │ │Pool 8    │ │ 5 tools  │   │    │
│  │  └──────────┘ └──────────┘ └──────────┘ └──────────┘   │    │
│  │                                                          │    │
│  │  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐   │    │
│  │  │ Quality  │ │ Session  │ │ NLP      │ │Intelligence│  │    │
│  │  │ 5 tools  │ │ 6 tools  │ │ 4 tools  │ │ 5 tools   │  │    │
│  │  └──────────┘ └──────────┘ └──────────┘ └──────────┘   │    │
│  └─────────────────────────────────────────────────────────┘    │
│                              │                                   │
│                              ▼                                   │
│                    ┌─────────────────┐                          │
│                    │  ARGUS BRAIN    │                          │
│                    │  (Backend API)  │                          │
│                    └─────────────────┘                          │
└─────────────────────────────────────────────────────────────────┘
```

### 2.2 MCP Tool Inventory by Category

#### Discovery & Analysis (12 tools)
| Tool | Purpose | Backend Endpoint |
|------|---------|------------------|
| `argus_discover` | Auto-discover testable surfaces | `POST /api/discover` |
| `argus_analyze` | Analyze codebase structure | `POST /api/analyze` |
| `argus_coverage_gaps` | Find untested code paths | `GET /api/coverage/gaps` |
| `argus_what_to_test` | Prioritized test suggestions | `POST /api/suggest` |
| `argus_risk_scores` | Calculate change risk | `POST /api/risk/score` |
| `argus_quality_score` | Project quality metrics | `GET /api/quality/score` |

#### Test Execution (8 tools)
| Tool | Purpose | Backend Endpoint |
|------|---------|------------------|
| `argus_test` | Execute single test | `POST /api/execute` |
| `argus_batch_generate` | Generate test suite | `POST /api/generate/batch` |
| `argus_generate_test` | Generate from spec | `POST /api/generate` |
| `argus_recording_to_test` | Convert recording | `POST /api/recording/convert` |
| `argus_test_from_event` | Test from failure | `POST /api/test/from-event` |
| `argus_test_review` | Review test quality | `POST /api/test/review` |

#### Self-Healing (9 tools)
| Tool | Purpose | Backend Endpoint |
|------|---------|------------------|
| `argus_healing_config` | Get healing settings | `GET /api/healing/config` |
| `argus_healing_patterns` | List learned patterns | `GET /api/healing/patterns` |
| `argus_healing_stats` | Healing success metrics | `GET /api/healing/stats` |
| `argus_healing_review` | Review healing suggestions | `POST /api/healing/review` |
| `argus_heal` | Trigger healing | `POST /api/heal` |

#### Intelligence Layer (5 tools)
| Tool | Purpose | Backend Endpoint |
|------|---------|------------------|
| `argus_ask` | Natural language query | `POST /api/chat` |
| `argus_agent` | Multi-agent orchestration | `POST /api/agent/invoke` |
| `argus_extract` | Extract patterns | `POST /api/extract` |

### 2.3 Data Flow: MCP → Backend

```
┌──────────────────────────────────────────────────────────────┐
│                     AI AGENT (Claude Code)                    │
│                                                               │
│   Agent has: • Filesystem MCP  • GitHub MCP  • Playwright MCP │
│              • Argus MCP (our gateway)                        │
└───────────────────────────┬───────────────────────────────────┘
                            │
                            ▼
┌───────────────────────────────────────────────────────────────┐
│                      ARGUS MCP SERVER                         │
│                                                               │
│   1. Receive tool call from agent                             │
│   2. Add authentication (API key or OAuth2 token)             │
│   3. Transform request to Brain API format                    │
│   4. Forward to Argus Brain backend                           │
│   5. Transform response back to MCP format                    │
│   6. Return to agent                                          │
│                                                               │
│   Storage: • Durable Objects (session state)                  │
│            • KV Namespace (caching)                           │
│            • R2 (screenshots, artifacts)                      │
└───────────────────────────┬───────────────────────────────────┘
                            │
                            ▼
┌───────────────────────────────────────────────────────────────┐
│                      ARGUS BRAIN (Backend)                    │
│                                                               │
│   478+ API endpoints across 57 modules                        │
│   Multi-tier intelligence: Cache → Precomputed → Vector → LLM │
│   40+ database tables in Supabase                             │
└───────────────────────────────────────────────────────────────┘
```

### 2.4 Intelligence Results Accessible via MCP

| Intelligence Type | MCP Tool | Backend Source | Latency |
|-------------------|----------|----------------|---------|
| Similar failure patterns | `argus_healing_patterns` | Cognee vector search | ~200ms |
| Test impact matrix | `argus_risk_scores` | Precomputed (Flink) | ~50ms |
| Healing suggestions | `argus_healing_review` | 4-layer cache | ~100ms |
| Code context | `argus_analyze` | Cognee knowledge graph | ~500ms |
| Coverage gaps | `argus_coverage_gaps` | Precomputed | ~50ms |

---

## Zone 3: Shared Features & Real-Time Sync

Features that must be synchronized between Dashboard and MCP.

### 3.1 Shared Data Model

```
┌─────────────────────────────────────────────────────────────────┐
│                    SHARED DATA (Zone 3)                          │
│                                                                  │
│  ┌──────────────────────────────────────────────────────────┐   │
│  │                     SUPABASE                              │   │
│  │                                                           │   │
│  │  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐       │   │
│  │  │  Projects   │  │   Tests     │  │  Results    │       │   │
│  │  │  (shared)   │  │  (shared)   │  │  (shared)   │       │   │
│  │  └─────────────┘  └─────────────┘  └─────────────┘       │   │
│  │                                                           │   │
│  │  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐       │   │
│  │  │  Healing    │  │  Coverage   │  │ Intelligence│       │   │
│  │  │  Patterns   │  │   Data      │  │ Precomputed │       │   │
│  │  └─────────────┘  └─────────────┘  └─────────────┘       │   │
│  └──────────────────────────────────────────────────────────┘   │
│                              │                                   │
│           ┌──────────────────┼──────────────────┐               │
│           ▼                  ▼                  ▼               │
│    ┌───────────┐      ┌───────────┐      ┌───────────┐         │
│    │ Dashboard │      │    MCP    │      │  Flink    │         │
│    │  (read/   │      │  (read/   │      │  (write)  │         │
│    │   write)  │      │   write)  │      │           │         │
│    └───────────┘      └───────────┘      └───────────┘         │
└─────────────────────────────────────────────────────────────────┘
```

### 3.2 Shared Tables (40+ in Supabase)

| Table | Dashboard | MCP | Sync Requirement |
|-------|-----------|-----|------------------|
| `projects` | Create/Update | Read | Eventually consistent |
| `tests` | CRUD | Create/Read | Real-time on create |
| `test_results` | Read | Create | Real-time streaming |
| `test_runs` | CRUD | Create/Read | Real-time status |
| `healing_patterns` | Read | Create/Update | Background sync |
| `healing_outcomes` | Read | Create | Batch sync |
| `coverage_data` | Read | Create | Background (Flink) |
| `intelligence_precomputed` | Read | Read | Background (Flink) |
| `failure_patterns` | Read | Read | Background (Flink) |
| `browser_pool_instances` | Read | Create/Update | Real-time health |

### 3.3 Real-Time Sync Requirements

#### Critical: Sub-second Sync
| Data Type | Protocol | Direction | Use Case |
|-----------|----------|-----------|----------|
| Test execution status | SSE | Backend → Both | Live progress |
| Browser pool health | WebSocket | Pool → Both | Capacity planning |
| Healing in progress | SSE | Backend → Both | UI feedback |

#### Important: 5-second Sync
| Data Type | Protocol | Direction | Use Case |
|-----------|----------|-----------|----------|
| Test results | Supabase Realtime | DB → Both | Result display |
| Coverage updates | Supabase Realtime | DB → Both | Dashboard refresh |
| Quality scores | Supabase Realtime | DB → Both | Metric display |

#### Background: 15-minute Sync
| Data Type | Protocol | Direction | Use Case |
|-----------|----------|-----------|----------|
| Precomputed analytics | Flink → Supabase | Flink → DB | Dashboard charts |
| Failure clusters | Flink → Supabase | Flink → DB | Pattern analysis |
| Test impact matrix | Flink → Supabase | Flink → DB | Change impact |

### 3.4 Real-Time Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                     REAL-TIME LAYER (Zone 3)                     │
│                                                                  │
│  ┌──────────────────────────────────────────────────────────┐   │
│  │                   EVENT SOURCES                           │   │
│  │                                                           │   │
│  │  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐       │   │
│  │  │  Redpanda   │  │  Supabase   │  │  Browser    │       │   │
│  │  │   Kafka     │  │  Realtime   │  │   Pool      │       │   │
│  │  │  8 topics   │  │  PG notify  │  │  WebSocket  │       │   │
│  │  └──────┬──────┘  └──────┬──────┘  └──────┬──────┘       │   │
│  │         │                │                 │              │   │
│  │         └────────────────┼─────────────────┘              │   │
│  │                          ▼                                │   │
│  │                 ┌─────────────────┐                       │   │
│  │                 │   SSE Gateway   │                       │   │
│  │                 │  /api/stream/*  │                       │   │
│  │                 └────────┬────────┘                       │   │
│  │                          │                                │   │
│  │           ┌──────────────┼──────────────┐                │   │
│  │           ▼              ▼              ▼                │   │
│  │    ┌───────────┐  ┌───────────┐  ┌───────────┐          │   │
│  │    │ Dashboard │  │    MCP    │  │  Webhooks │          │   │
│  │    │   Client  │  │   Client  │  │  (Slack)  │          │   │
│  │    └───────────┘  └───────────┘  └───────────┘          │   │
│  └──────────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────┘
```

### 3.5 Kafka Topics (Event-Driven Backbone)

| Topic | Producer | Consumers | Retention |
|-------|----------|-----------|-----------|
| `argus.codebase.ingested` | API | Cognee Worker | 7 days |
| `argus.test.executed` | Test Runner | Flink, Cognee | 30 days |
| `argus.test.failed` | Test Runner | Healer, Flink | 30 days |
| `argus.healing.requested` | Dashboard/MCP | Healer Agent | 14 days |
| `argus.healing.completed` | Healer | Dashboard, MCP | 30 days |
| `argus.integration.github.pr` | GitHub Webhook | Cognee Worker | 30 days |
| `argus.integration.sentry` | Sentry Webhook | Cognee Worker | 14 days |
| `argus.patterns.failure-cluster` | Flink | Healer, Dashboard | 30 days |

---

## Authentication & Authorization Matrix

### 4.1 Auth Mechanisms by Zone

| Zone | Auth Method | Token Type | Scope |
|------|-------------|------------|-------|
| Dashboard | Clerk | JWT (HttpOnly cookie) | Full access |
| MCP | API Key | `argus_sk_*` | Per-org scoped |
| MCP (OAuth2) | Device Flow | Bearer token | Per-user scoped |
| Internal | Service Role | Supabase key | Admin access |

### 4.2 Unified Identity Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                    IDENTITY LAYER                                │
│                                                                  │
│  ┌──────────────────────────────────────────────────────────┐   │
│  │                    CLERK (IdP)                            │   │
│  │                                                           │   │
│  │  • User database (email, password, MFA)                   │   │
│  │  • Organization management                                │   │
│  │  • OAuth2 provider (Google, GitHub SSO)                   │   │
│  │  • JWT tokens for Dashboard                               │   │
│  └──────────────────────────────────────────────────────────┘   │
│                              │                                   │
│                              ▼                                   │
│  ┌──────────────────────────────────────────────────────────┐   │
│  │                    SUPABASE RLS                           │   │
│  │                                                           │   │
│  │  • `has_org_access(org_id)` - Multi-tenant isolation      │   │
│  │  • `has_project_access(project_id)` - Project scoping     │   │
│  │  • `current_user_id()` - User context propagation         │   │
│  │  • Service role bypass for Flink jobs                     │   │
│  └──────────────────────────────────────────────────────────┘   │
│                              │                                   │
│           ┌──────────────────┼──────────────────┐               │
│           ▼                  ▼                  ▼               │
│    ┌───────────┐      ┌───────────┐      ┌───────────┐         │
│    │ Dashboard │      │    MCP    │      │  Backend  │         │
│    │  (Clerk   │      │  (API Key │      │  (Service │         │
│    │   JWT)    │      │   +OAuth) │      │   Role)   │         │
│    └───────────┘      └───────────┘      └───────────┘         │
└─────────────────────────────────────────────────────────────────┘
```

---

## Identified Gaps & Recommendations

### 5.1 Current Gaps

| Gap | Impact | Priority |
|-----|--------|----------|
| No MCP → Dashboard push channel | MCP changes not reflected in real-time | High |
| Pattern distribution one-way | Dashboard patterns not pushed to MCP cache | Medium |
| No conflict resolution | Concurrent edits from both zones | Medium |
| Missing webhook for MCP events | Dashboard can't subscribe to MCP actions | Low |

### 5.2 Recommended Sync Protocol

```
┌─────────────────────────────────────────────────────────────────┐
│               PROPOSED: BIDIRECTIONAL SYNC                       │
│                                                                  │
│  ┌──────────────┐                      ┌──────────────┐         │
│  │  Dashboard   │◄────── SSE ─────────►│     MCP      │         │
│  │              │                      │              │         │
│  │              │◄─── Supabase ────────►│              │         │
│  │              │     Realtime         │              │         │
│  │              │                      │              │         │
│  │              │◄─── Webhooks ────────►│              │         │
│  │              │   (change events)    │              │         │
│  └──────────────┘                      └──────────────┘         │
│                                                                  │
│  Sync Protocol:                                                  │
│  1. MCP writes to Supabase → PG NOTIFY triggers                  │
│  2. Dashboard subscribes via Supabase Realtime                   │
│  3. Dashboard writes → Same mechanism for MCP                    │
│  4. Webhook fallback for critical events                         │
└─────────────────────────────────────────────────────────────────┘
```

---

## Summary: Feature Isolation Matrix

| Capability | Zone 1 (Dashboard) | Zone 2 (MCP) | Zone 3 (Shared) |
|------------|-------------------|--------------|-----------------|
| **User Management** | ✅ Full | ❌ None | - |
| **Integration OAuth** | ✅ Full | ❌ None | - |
| **Billing** | ✅ Full | ❌ None | - |
| **Test Execution** | ✅ Trigger | ✅ Trigger | ⚡ Results |
| **Test Generation** | ✅ UI | ✅ API | ⚡ Storage |
| **Healing** | ✅ Review | ✅ Auto | ⚡ Patterns |
| **Analytics** | ✅ Visualize | ✅ Query | ⚡ Data |
| **Coverage** | ✅ View | ✅ Query | ⚡ Metrics |
| **Reports** | ✅ Generate | ✅ Fetch | ⚡ Storage |
| **Real-time Status** | ✅ Stream | ✅ Poll/Stream | ⚡ Events |

---

## Conclusion

The Argus 3-Zone Architecture ensures:

1. **Dashboard (Zone 1)**: Full control over configuration, integrations, and human-in-the-loop workflows
2. **MCP (Zone 2)**: Fast, AI-optimized access to intelligence results without local processing
3. **Shared (Zone 3)**: Consistent data model with real-time sync for seamless user experience

The **backend is the powerhouse** - all intelligence (multi-model routing, vector search, precomputed analytics) runs server-side. MCP is a simple, fast gateway that surfaces these capabilities to AI agents.
