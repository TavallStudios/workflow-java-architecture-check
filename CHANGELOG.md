# Changelog

All notable changes to the `java-architecture-check` workflow source repository are documented here. Versions follow the `IWorkflowMarketplace.Version` (major.minor.patch) model owned by Tavall Workflows.

## 1.0.0 — 2026-11-17

### Added

- Published canonical source repository `TavallStudios/workflow-java-architecture-check` for the `java-architecture-check` pilot workflow, per the merged published-workflow-repository rule (tavall-docs#151).
- `README.md` with usage, behavior, real end-to-end output, and links to the authoritative runner contract.
- `workflow.json` source descriptor referencing the owner-defined `IWorkflowRunner` / `tavall workflow run` contract; no competing manifest format.
- Apache-2.0 license (orchestrator decision on workflows#20; permissive, explicit patent grant).
- Verification reference to tavall-workflows PR #23: exact-head compile PASS, `:workflows-cli` 11 tests PASS, real CLI e2e PASS (2 checks) at head `c0fadf3`.
