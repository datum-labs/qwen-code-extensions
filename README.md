# Qwen Code Extensions

Qwen Code extensions for the Datum Cloud platform. Adapted from [claude-code-plugins](https://github.com/datum-cloud/claude-code-plugins).

[![License: Apache-2.0](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](LICENSE)

## Extensions

### datum-platform

Kubernetes platform engineering automation with aggregated API servers, controller patterns, GitOps deployment, and platform capabilities.

**Components:**
- 8 agents (api-dev, code-reviewer, frontend-dev, operational-reviewer, plan, sre, tech-writer, test-engineer)
- 8 commands (corrections, deploy, discover, evolve, patterns, pipeline, review, trends)
- 27 skills (C4 diagrams, capability integrations, Go conventions, K8s patterns, CI/CD, etc.)
- 1 script (auto-validate)

### datum-gtm

Go-to-market automation with commercial strategy, product discovery, and customer support tools.

**Components:**
- 4 agents (commercial-strategist, gtm-comms, product-discovery, support-triage)
- 1 command (discover)
- 3 skills (commercial models, discovery, GTM templates)

### milo-activity

Query audit logs, investigate incidents, and author ActivityPolicies using the Milo Activity service.

**Components:**
- 2 agents (incident-investigator, policy-author)
- 7 skills (activity summary, audit, policy authoring, incident investigation, preview, status)
- Requires the `activity` binary as an MCP server

## Installation

### From local path

```bash
qwen extensions install /path/to/qwen-code-extensions/extensions/datum-platform
qwen extensions install /path/to/qwen-code-extensions/extensions/datum-gtm
qwen extensions install /path/to/qwen-code-extensions/extensions/milo-activity
```

### From git repository

```bash
qwen extensions install https://github.com/datum-cloud/qwen-code-extensions#datum-platform
qwen extensions install https://github.com/datum-cloud/qwen-code-extensions#datum-gtm
qwen extensions install https://github.com/datum-cloud/qwen-code-extensions#milo-activity
```

## Migration from Claude Code Plugins

This repository adapts the [claude-code-plugins](https://github.com/datum-cloud/claude-code-plugins) marketplace for Qwen Code. The underlying content (skills, agents, commands) retains its original license (MIT or AGPL-3.0 as noted per plugin).

Key changes:
- `claude-plugin.json` → `qwen-extension.json`
- Agent model hints (Opus/Sonnet/Haiku) → `model: inherit`
- Claude tool names (Read/Write/Edit) → Qwen tool names (read_file/write_file/edit)
- `.claude/` paths → `.qwen/` paths
- `permissionMode: plan` → `approvalMode: plan`
- PostToolUse hook → Validation skill
- No hooks system in Qwen — validation is skill-invoked
