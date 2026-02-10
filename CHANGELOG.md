# Changelog

All notable changes to the Skopaq Documentation will be documented in this file.

## [1.3.0] - 2026-01-30

### Added

- **3-Zone Architecture** documentation (`technical/zone-architecture.md`)
  - PhD-level systems architecture analysis
  - Zone 1: Dashboard-only features (35+ cloud-exclusive capabilities)
  - Zone 2: MCP-accessible features (73 gateway tools inventory)
  - Zone 3: Shared features and real-time sync requirements
  - Feature isolation matrix across all zones
  - Authentication & authorization matrix
  - Identified gaps and sync protocol recommendations
  - Complete data flow diagrams

## [1.2.0] - 2026-01-29

### Added

- **MCP & Plugin Integration Architecture** documentation (`technical/mcp-integration.md`)
  - Complete inventory of 73 MCP tools by category
  - 27 AI agents with implementation status
  - 13+ integration platforms documented
  - Knowledge graph architecture (Cognee ECL pipeline)
  - Cross-source correlation patterns (5 implemented)
  - Tiered deployment model (Free → Air-Gapped)
  - Local MCP server architecture for privacy mode
  - Linear sprint status and recent completions

## [1.1.0] - 2026-01-29

### Added

- **Observability Architecture** documentation (`technical/observability.md`)
  - Langfuse LLM tracing integration
  - Prometheus + Grafana monitoring stack
  - Kubernetes ServiceMonitors
  - Alerting rules configuration
  - Cloudflare Tunnel for secure access

### Changed

- **Implementation Status** updated to 80% readiness (from 75%)
  - Added "Recent Updates (January 29, 2026)" section
  - Documented critical bug fixes:
    - Supervisor RunnableConfig type fix
    - Chat API input validation hardening
  - Added Orchestration Layer status table
  - Added Kubernetes Monitoring files inventory
  - Comprehensive validation results (96.7% pass rate)
  - Updated unique advantages with new capabilities

### Fixed

- Supervisor orchestration now works correctly (LangGraph config injection)
- Chat API no longer crashes on malformed input (proper 422 validation)

## [1.0.0] - 2026-01-17

### Added

- Initial unified documentation structure
- Release-please workflow for automated versioning
- Claude Code review workflow for PR quality
- Architecture documentation section
  - Skopaq Brain architecture
  - System diagrams
- API Reference with OpenAPI specification
- User guides
  - Quickstart guide
  - Workflows documentation
  - Contributing guide
- Product documentation
  - Features overview
  - Public roadmap
  - Vision document
- Deployment documentation
  - Deployment guide
  - Browser pool migration
  - FAQ

### Changed

- Reorganized navigation structure for better discoverability
- Updated Technical section with latest architecture

### Internal

- Consolidated documentation from multiple repositories
- Separated public docs from internal strategy documents
