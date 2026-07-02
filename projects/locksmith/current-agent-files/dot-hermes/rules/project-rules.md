# Project Rules — Locksmith

Стабильные правила, которые агент **всегда** применяет.

## Принципы
- **Think before coding:** явно назвать предположения, неоднозначности и tradeoffs.
- **Simplicity first:** не добавлять speculative flexibility.
- **Surgical changes:** менять только то, что связано с задачей.
- **Spec-Driven Development:** перед любым изменением кода прочитать spec компонента в `docs/` и обновить при необходимости.
- **Перед визуальной проверкой — git push** (симулятор на Mac, не на сервере).

## Рабочий цикл
1. Прочитать current-задачу в `plan/current.md`.
2. Прочитать spec нужных компонентов в `docs/`.
3. Сделать план (≤ 5 шагов).
4. Выполнить маленькими порциями.
5. Запустить `make lint` → `make test-all` → `make build` или единый `make check`.
6. Зафиксировать след (commit + evidence).

## Стоп-критерии
- **Успех:** критерии выполнены, тесты проходят, билд собирается.
- **Блокировка:** не хватает данных, доступа или решения человека.
- **Unsafe:** action нарушает security policy.
- **Budget:** превышен лимит попыток/времени.
- **Repeated failure:** одинаковая ошибка > 2 раз.

## Tool policy
- Все destructive actions — с подтверждением.
- Секреты — вне контекста (env vars, vault).
- Результаты tool calls — структурированно, source+date.
- Каждый tool call логируется в `.hermes/observability/audit-log.md`.

## Специфика проекта
- **PlayDate SDK 3.0.3** — Y-ось вниз, `gfx.fillPolygon` не работает (нужен `playdate.geometry.polygon.new`).
- **SDD (Spec-Driven Development):** обязательно читать `docs/<component>.md` перед изменениями.
- **Makefile:** `make test-all` (lua тесты), `make build` (pdc), `make lint`/`make spec` (spec-review).
- **Пре-коммит:** `make check` (test-all + lint + build).
- **Визуал:** итеративная настройка (2-7 px), дискретные анимации, push-фидбек на Mac.
- **Ошибки → документировать в AGENTS.md** — любая runtime-ошибка или баг API немедленно фиксируется в Pitfalls.
