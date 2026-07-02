# Security Policy — Locksmith

## Tool allowlist
- **Allowed:** shell (make, git, lua, pdc), filesystem (read/write in scope), git, web (docs)
- **Denied:** rm -rf without confirmation, curl | bash, eval on untrusted input, changing `source/` files that aren't in current task scope

## Approval gates
- Destructive actions (delete files, rewrite git history) → confirm with user
- Changes outside `source/`, `docs/`, `scripts/` → confirm with user
- Modifying `.git` → confirm with user

## Secrets
- Secrets ONLY in env vars or vault — NEVER in model context.
- Do not read or log secrets.
- No API keys are used in this project.

## Audit
- Each tool call logged: timestamp, tool, args (redacted), result.
- Log at `.hermes/observability/audit-log.md`.
