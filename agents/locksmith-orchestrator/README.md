# Locksmith Orchestrator — пользовательский README

## Что это

**Locksmith Orchestrator** — управляющий агент для разработки игры Locksmith
на PlayDate. Он не пишет код сам, а координирует работу специализированных
агентов по правилам Spec-Driven Development.

## Как пользоваться

1. **Опиши задачу** — просто скажи, что нужно сделать.
   - *"Crank feels sluggish at low speeds"*
   - *"Add a new Endless item — Brass Gear"*
   - *"Adventure intro needs a new background image"*
   - *"Build is broken"*
   - *"Add a confirmation before resetting highscores"*

2. **Orchestrator маршрутизирует** — определит, какие компоненты затрагивает
   задача, прочитает нужные specs, обновит их при необходимости и отправит
   задачу правильному специалисту.

3. **Специалист работает** — реализует изменение в своей зоне ответственности.

4. **Orchestrator проверяет** — запустит тесты, линтер и сборку. Если что-то
   падает, вернёт на доработку. Если находит новую ошибку — запишет в
   документацию.

5. **Ты получаешь результат** — отчёт о том, что было сделано, какие specs
   обновлены, прошли ли тесты и сборка.

## Каких специалистов вызывает

| Область | Кто делает |
|---------|-----------|
| Физика замка, механика пинов | Crank Feel / Micro-Mechanics Specialist |
| Отрисовка, рендеринг | Playdate Pixel UI / Renderer Specialist |
| Управление (crank, D-pad, A/B) | Crank Feel / Micro-Mechanics Specialist |
| Экраны, меню, HUD | Playdate Pixel UI / Renderer Specialist |
| Игровые режимы (Speedrun/Adventure/Endless) | Lua Gameplay Engineer |
| Экономика Endless, баланс | Endless Economy Specialist |
| Сохранения | Lua Gameplay Engineer |
| Музыка, звуки | Audio/Music Feedback Specialist |
| Сюжет Adventure | Adventure State Machine Specialist |
| Визуальные ассеты | Visual Art / Asset Production Specialist |
| Маркетинг, devlog | Devlog/Community Marketing Specialist |
| Расхождение spec и кода | Spec/Contract Guardian |
| Сборка, тесты | Toolchain/Build Gatekeeper |
| Создание новых агентов | Agent Architect / Crew Builder |

## Что Orchestrator НЕ делает

- ✗ Не пишет игровой код (только координирует)
- ✗ Не анализирует картинки без твоего разрешения
- ✗ Не называет игру Safe Cracker
- ✗ Не переименовывает Сайласа Крейна
- ✗ Не хардкодит путь к PlayDate SDK
- ✗ Не генерирует procedural art вместо asset pipeline

## Что важно помнить

- Любое изменение поведения/UX/API/правил — сначала обновляется spec, потом код.
- Перед каждым изменением кода запускаются тесты (`make test-all`).
- После каждого изменения — тесты, линтер и сборка (`make check`).
- Любая новая ошибка записывается в `AGENTS.md` в раздел pitfalls.
