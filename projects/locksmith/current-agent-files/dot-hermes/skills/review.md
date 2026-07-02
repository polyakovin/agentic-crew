# Code Review — Locksmith

## Когда использовать
- Перед коммитом изменений (pre-commit review).
- После имплементации новой фичи.
- При рефакторинге.
- Перед git push.

## Алгоритм

### 1. Проверь архитектуру
- **safe.lua** — pure logic, без gfx.*, без ссылок на renderer/game/input/ui.
- **renderer.lua** — pure drawing, без мутации game state.
- **input.lua** — input capture, без safe/renderer/game/ui refs.
- **ui.lua** — static screens, без game logic.
- **mode modules** — orchestrators, делегируют logic/input/drawing, не импортируют main.lua.
- **main.lua** — dispatcher, создаёт режимы и вызывает playdate.update.

### 2. Проверь spec compliance
```bash
make spec
```
- Убедись, что spec-review.lua проходит.
- Если spec менялся, проверь что `docs/<component>.md` актуален.

### 3. Проверь test coverage
- Новая механика → есть unit/integration coverage в `scripts/test_*.lua` или headless tests.
- Старые тесты всё ещё проходят (`make test-all`).

### 4. Проверь build
```bash
make build
```

### 5. Проверь ошибки API PlayDate
- `gfx.fillPolygon` — нет, только `playdate.geometry.polygon.new`.
- `gfx.setColor` не перед `image:draw()` или `drawTextAligned()`.
- Y-ось: «вверх» = уменьшение Y.

### 6. Проверь документацию
- AGENTS.md — все новые pitfalls зафиксированы.
- README/TODO — актуальны.

## Критерии успеха
- `make spec` проходит.
- `make test-all` — все тесты зелёные.
- `make build` — pdx собирается.
- Архитектурные ограничения соблюдены (чистые/нечистые модули).
- AGENTS.md в актуальном состоянии.

## Pitfalls
- Легко забыть обновить AGENTS.md с новым pitfall — проверяй осознанно.
- spec-review может отставать от кода — запускай `make spec` явно.
- **Если код меняет API компонента — spec ДОЛЖЕН быть обновлён до кода** (правило SDD).
