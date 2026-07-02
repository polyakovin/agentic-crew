# TDD (Spec-Driven) — Locksmith

## Когда использовать
- Добавление новой механики в safe.lua (pin physics, crank, tryCatch).
- Изменение контрактов между компонентами (safe → mode → renderer).
- Рефакторинг pure logic с сохранением поведения.

## Алгоритм

### 1. Прочитай spec
```bash
cat docs/safe.md        # для изменений в safe.lua
cat docs/game.md        # для изменений в режимах и dispatcher
cat docs/manifest.json  # контракты и зависимости
```

### 2. Напиши spec-first
Если меняешь API компонента — сначала обнови `docs/<component>.md`:
- сигнатуры функций
- return values
- зависимости (какие модули импортирует/не импортирует)

### 3. Напиши тест в `scripts/test_safe.lua`
Тесты на Lua без внешних зависимостей (только PlayDate stubs):
```lua
-- test_xxx()
assert(safe.new() ~= nil)
assert(safe:getPinCount() == 6)
```

### 4. Запусти тесты
```bash
make test-all
```

### 5. Имплементируй код

### 6. Запусти spec-review
```bash
make spec
```

### 7. Собери билд
```bash
make build
```

## Критерии успеха
- Новые тесты проходят.
- Старые тесты не сломаны.
- `make spec` проходит (spec-review.lua).
- `make build` собирает .pdx без ошибок.

## Pitfalls
- PlayDate Lua API **не поддерживает** `math.random` с seed так же, как PUC Lua — используй `playdate.math.random()`.
- Тесты на safe.lua используют stubs из `scripts/test_stubs.lua` — проверь, что stubs покрывают нужные функции.
- `safe.lua` не должен импортировать `gfx.*` — это pure logic.
