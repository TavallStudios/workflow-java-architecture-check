# Java Architecture Check

A runnable Tavall Workflow that verifies Java source in a repository worktree against Tavall's CLI architecture rules, with an advisory `java_gate` quick check.

This repository is the canonical source and documentation for the `java-architecture-check` workflow. The workflow runs through the Tavall Workflows runner contract owned by [TavallStudios/tavall-workflows](https://github.com/TavallStudios/tavall-workflows); this repository does not define a new execution format and does not imply a deployed Workflows runtime or MCP endpoint.

[![License: Apache-2.0](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](LICENSE)

## Why Java Architecture Check

- Catch CLI architecture rule violations early, before code review, using the same rules Tavall applies to its own modules.
- Produce one typed, machine-readable report with a clear pass/fail result and per-check detail.
- Run against any repository worktree without requiring deployed Tavall infrastructure.

## Features

- `java-gate-quick` — advisory `java_gate quick` check on the target worktree (never a verification receipt).
- `architecture-rules/tavall-cli` — applies the canonical CLI architecture rule that concrete `CLICommand` subclasses must be registered through an `ICLIBootstrap`.
- One typed `IWorkflowRunner.WorkflowRunReport` result with per-check status and detail.

## Quick Start

### Install / Dependency

This workflow executes through the `tavall` single CLI framework provided by Tavall Workflows. No separate runtime service is required. The runnable contract (`IWorkflowRunner`) and the `workflow run` command live in `TavallStudios/tavall-workflows` (`workflows-api` and `workflows-cli` modules).

### Minimal Example

```text
tavall workflow run java-architecture-check --worktree /path/to/repo/worktree --base origin/main
```

The command discovers the workflow through the `ICLIBootstrap` SPI and adapts input to the typed `IWorkflowRunner` capability, then renders the returned report.

## How It Works

The workflow maps a `WorkflowRunRequest` (workflow id, worktree path, base ref) to `JavaArchitectureValidationRunner`, which runs the Tavall-owned checks against the worktree and returns one `WorkflowRunReport`. External tool invocation is isolated behind a `ToolInvoker` boundary; the runner, rules and report assembly are real production code. See [the owner contract](https://github.com/TavallStudios/tavall-workflows/tree/main/workflows-api) and [the CLI adapter](https://github.com/TavallStudios/tavall-workflows/tree/main/workflows-cli) for the authoritative implementation.

## Example / Demo

Real end-to-end output against a Tavall Workflows worktree:

```text
java-architecture-check: PASS (2 checks)

Details:
  workflowId          : java-architecture-check
  passed              : true
  java-gate-quick     : PASS: state=OK; advisory only (java_gate quick, not verification)
  architecture-rules/tavall-cli: PASS: no violations
```

## Project Structure

```text
workflow-java-architecture-check/
├── README.md         # This document
├── workflow.json     # Source descriptor referencing the owner-defined execution contract
├── CHANGELOG.md      # Version history
└── LICENSE           # Apache-2.0
```

## Documentation

- Workflow runner contract (`IWorkflowRunner`) — [tavall-workflows/workflows-api](https://github.com/TavallStudios/tavall-workflows/tree/main/workflows-api)
- CLI adapter and pilot runner — [tavall-workflows/workflows-cli](https://github.com/TavallStudios/tavall-workflows/tree/main/workflows-cli)
- Verification evidence — [tavall-workflows PR #23](https://github.com/TavallStudios/tavall-workflows/pull/23)

## Requirements / Compatibility

- Java toolchain 25 and the Tavall `./gradlew` wrapper, as declared by `TavallStudios/tavall-workflows`.
- A local repository worktree to check.

## Building From Source

The runnable Java source is owned and built in `TavallStudios/tavall-workflows` (`workflows-api` + `workflows-cli`); this repository publishes the source descriptor, documentation and version history only. Build instructions live in that repository.

## Contributing

Issues and changes to the executable contract belong in [TavallStudios/tavall-workflows](https://github.com/TavallStudios/tavall-workflows). Changes to this repository's documentation, descriptor or changelog are welcome through pull requests.

## License

Apache-2.0. See [LICENSE](LICENSE).
