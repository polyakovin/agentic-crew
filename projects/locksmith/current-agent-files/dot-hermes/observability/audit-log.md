# Audit Log — Locksmith

## Format
```
| Timestamp | Tool | Args | Result |
|-----------|------|------|--------|
| 2025-01-01T12:00:00Z | read_file | path=source/safe.lua | OK |
```

Log entries prefixed with the date of the session.
Tool args are redacted for secrets.
