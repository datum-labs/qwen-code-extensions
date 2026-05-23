---
name: validation
description: >
  Run automated validation scripts after modifying code files. Use after
  writing or editing Go files, Kustomize manifests, Dockerfiles, or
  Kubernetes configuration. Validates code style, security, build
  correctness, and test coverage. Run after every Write or Edit operation.
---

# Validation Skill

Run automated validation after making code changes. This skill replaces
the PostToolUse hook that previously ran validation automatically.

## When to Run

After **every** Write or Edit operation, run the appropriate validation:

- **Go files** (`*.go`): Import checking, boilerplate, test naming, type validation
- **Kustomize files** (`kustomization.yaml`, `*.yaml` in `config/`): Build validation, security checks
- **Dockerfiles**: Container security checks
- **GitHub Actions** (`.github/workflows/`): Workflow syntax validation

## How to Run

### For Go Files

```bash
# Run the auto-validation script
./extensions/datum-platform/scripts/auto-validate.sh go <file-path>
```

Or run the individual checks:

```bash
# Import ordering
./extensions/datum-platform/skills/go-conventions/scripts/check-imports.sh

# Test naming conventions
./extensions/datum-platform/skills/go-conventions/scripts/check-test-naming.sh

# Boilerplate (license headers, copyright)
./extensions/datum-platform/skills/go-conventions/scripts/check-boilerplate.sh

# Type validation (Kubernetes types)
./extensions/datum-platform/skills/k8s-apiserver-patterns/scripts/validate-types.sh
```

### For Kustomize/Infrastructure Files

```bash
# Kustomize build validation
./extensions/datum-platform/skills/kustomize-patterns/scripts/validate-kustomize.sh

# Security checks (container security, RBAC, etc.)
./extensions/datum-platform/skills/kustomize-patterns/scripts/check-security.sh
```

### For All Changes

```bash
# Full build and test
task generate
task lint
task test
```

## Validation Output

If any validation fails:

1. **Read the error output** — understand what failed
2. **Fix the issue** — don't just suppress the error
3. **Re-run validation** — confirm it passes
4. **Do NOT proceed** until all validations pass

## Common Failures

### Import ordering

```bash
# Fix import ordering
gofmt -w <file.go>
```

### Missing boilerplate

```bash
# Add license headers
./extensions/datum-platform/skills/go-conventions/scripts/check-boilerplate.sh
```

### Kustomize build failure

```bash
# Test build
kubectl kustomize config/overlays/development
```

### Security check failure

Common fixes:
- Add `runAsNonRoot: true` to security context
- Add `readOnlyRootFilesystem: true`
- Add `drop: ["ALL"]` capabilities
- Set resource limits and requests

## Notes

- The `auto-validate.sh` script reads JSON input from stdin with file path information
- It selects the appropriate validators based on file type
- All validations must pass before declaring a change complete
