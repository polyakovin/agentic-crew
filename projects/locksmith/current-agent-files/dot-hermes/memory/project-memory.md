# Project Memory — Locksmith

## Basic info
- **Name:** Locksmith
- **Type:** PlayDate game (Lua)
- **Created:** 2025
- **Stack:** Lua, PlayDate SDK 3.0.3
- **GitHub:** https://github.com/polyakovin/playdate-safe-cracker
- **Author:** Игорь Поляков

## Conventions
- **Spec-Driven Development** — меняй spec (`docs/<component>.md`) перед кодом.
- **Architecture:** pure logic (safe.lua) → pure renderer (renderer.lua) → mode orchestrators → main dispatcher.
- **Make targets:** `make test-all` (unit), `make build` (pdc), `make lint`/`make spec` (spec-review), `make check` (full gate).
- **Визуал:** push на GitHub → симулятор на Mac у пользователя.
- **Pitfalls** — сразу в AGENTS.md (секция «PlayDate API Pitfalls»).

## Decisions
| Date | Decision | Rationale | Alternatives |
|------|----------|-----------|--------------|
| 2025 | Y-ось вниз, `gfx.fillPolygon` не используется | PlayDate SDK 3.0.3 требует `playdate.geometry.polygon.new` | |
| 2025 | SDD вместо TDD | Проект управляется через spec, тесты — проверка контрактов | |

## Preferences
- Только русский в общении.
- Перед отправкой картинок — git push.
- Агент не анализирует изображения без явной просьбы.
