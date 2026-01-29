# Changelog

All notable changes to the Argus Documentation will be documented in this file.

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
  - Argus Brain architecture
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
