# Codex Cloud Agent + Memory Bank (optimized)

Агент для Codex Cloud в стиле SDD: **Contracts → Tests → Code → ADR → Progress**,
с расширенным мемори-банком `.memory/` и строгими правилами автономности (0/1/2).

## Дерево
contracts/                 # Истина №1: OpenAPI/JSON Schema/интерфейсы
  VERSION.json             # Версионирование контрактов (SemVer, дата, миграции)
AGENTS.md                  # Правила для Codex (ядро + ссылки на плейбуки)
.memory/
  MISSION.md               # Миссия/ценность/границы (YAML front-matter)
  CONTEXT.md               # Факты среды, команды, Deprecation policy
  DECISIONS.md             # ADR-lite с front-matter (id/status/supersedes/…)
  TASKS.md                 # Мини-канбан с id/owner/dates
  PROGRESS.md              # Changelog (1 строка на событие) + front-matter
  GLOSSARY.md              # Термины/соглашения
  AUTONOMY.md              # Пороговые критерии 0/1/2 + CONSENT_WINDOW
  USECASES.md              # 5–10 ключевых сценариев + acceptance criteria
  REPORT_TEMPLATE.md       # Шаблон финального отчёта (текст)
  REPORT_SCHEMA.json       # JSON-схема отчёта
  INDEX.yaml               # Машиночитаемый индекс ADR/версий контрактов/дат
  WORKLOG.md               # Черновой ход работ до checkpoint
# LOCK файл создаётся динамически: .memory/LOCK.<taskId> (эфемерный)

## Ключевые идеи
- **Front-matter & INDEX** → детерминированное чтение памяти моделью.
- **Автономность с порогами** → Codex знает, когда спрашивать у пользователя.
- **SemVer + Deprecation policy** → управляемые изменения API.
- **USECASES + Acceptance** → уменьшаем гадание и переинтерпретации.
- **LOCK & WORKLOG** → без гонок и «дрейфа» канбана/прогресса.
- **REPORT.json** → машиночитаемый дайджест для автоматических проверок/ботов.

## Базовый цикл
1) Codex читает `AGENTS.md`, спрашивает автономность (0/1/2).  
2) Читает `.memory/*`, `contracts/*`, действует по Contracts-first & TDD.  
3) До checkpoint пишет шаги в `WORKLOG.md`; после checkpoint — синхронизирует `TASKS.md`/`PROGRESS.md`.  
4) При конфликте ADR — помечает устаревший как `superseded`.  
5) Перед PR/ответом — формирует отчёт: текст + `REPORT.json` по схеме.

## PR политика (кратко)
- Один PR — одна цель; unit+contract тесты зелёные; контракты/ADR/прогресс обновлены.
- В описании PR: цель, влияние на API/данные, риски, план отката, миграции.
