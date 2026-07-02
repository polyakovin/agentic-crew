# Debug — Locksmith

## Когда использовать
- Runtime ошибка при сборке или на симуляторе.
- Тест не проходит.
- Неверное визуальное поведение (смещение, цвета, перекрытие).

## Алгоритм

### 1. Воспроизведи ошибку
```bash
make test-all # unit tests
make build   # pdc сборка
```

### 2. Собери evidence
- Полный вывод ошибки (stack trace).
- На каком компоненте (`safe.lua`, `renderer.lua`, mode module, `main.lua`).
- Входные данные, при которых падает.

### 3. Root cause analysis
- Проверь Y-ось — не перепутано ли направление (Y растёт вниз).
- Проверь совместимость API (`gfx.fillPolygon` vs `playdate.geometry.polygon.new`).
- Проверь `gfx.setColor` + `image:draw()` — несовместимы (AGENTS.md → Pitfalls).
- Проверь `gfx.setColor` + `drawTextAligned()` — тот же баг.
- Проверь, что pure logic модули не импортируют gfx.
- Проверь, что `fillRect(y, h)` — последняя заполненная строка = y+h-1.

### 4. Исправь
- Минимальное изменение.
- После фикса: `make lint && make test-all && make build` или `make check`.

### 5. Документируй ошибку в AGENTS.md
Если это новый PlayDate API баг — добавь в секцию «PlayDate API Pitfalls».

## Критерии успеха
- `make test-all` — 0 failures.
- `make build` — 0 errors.
- `make spec` — spec-review passes.
- Ошибка зафиксирована в AGENTS.md (если это новый баг API).

## Pitfalls
- **Самая частая причина багов на PlayDate:** перепутанная Y-ось. Всегда проверяй знак перед визуальным смещением.
- **setColor → drawTextAligned:** всегда сбрасывай `gfx.setImageDrawMode(gfx.kDrawModeCopy)` перед рисованием текста после `setColor`.
- **fillPolygon не работает:** используй `playdate.geometry.polygon.new(x1,y1,x2,y2,...)` — координаты через запятую, последняя точка = первой.
- **image:draw() после setColor:** вместо `setColor` перед отрисовкой изображений используй `setImageDrawMode`.
