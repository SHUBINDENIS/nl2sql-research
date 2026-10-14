# NL2SQL на open-weight моделях — исследовательская часть ВКР

Text-to-SQL (NL → SQL) на открытых моделях Qwen2.5-Coder-7B-Instruct и Qwen3-Coder-30B-A3B-Instruct: schema linking → planner → генерация кандидатов → candidate selector → dialect handlers. Эксперименты на Spider 1.0, BIRD и семействе Spider 2.0 (Lite / Snow / DBT).

Репозиторий — моя часть парного дипломного проекта NL2BI (ВШЭ МИЭМ, 2026), выделенная с сохранением истории из ветки `experiments/denis` проекта [NL2BI-AI-assistant](https://github.com/petrussia/NL2BI-AI-assistant). BI-часть и веб-интерфейс остались в исходном репозитории. Сырые данные бенчмарков, файлы предсказаний и логи прогонов сюда не входят: данные скачиваются с официальных страниц бенчмарков.

## Главное

Сводка всех фаз с метриками и 95% доверительными интервалами (Wilson): **[RESEARCH_SUMMARY_FINAL.md](RESEARCH_SUMMARY_FINAL.md)**.

| Бенчмарк (FULL), EX | Бейзлайн B0 | Лучший в исследовании (B6_v7, Qwen2.5-Coder-7B) |
|---|---:|---:|
| Spider 1.0 dev (1034) | 72.5% | **76.8%** [74.1, 79.3] |
| BIRD Mini-Dev (500) | 20.4% | **38.8%** [34.6, 43.1] |

Отрицательные результаты зафиксированы наравне с положительными: например, planner без верификатора снижает EX, а реранкер ухудшает BIRD.

Spider 2.0, фаза 18 — schema-first планирование по закрытому словарю схемы (live-каталог → JSON-план → детерминированный рендер SQL → валидация и dry-run): [outputs/REPORT_SPIDER2_V18.md](outputs/REPORT_SPIDER2_V18.md).

## Структура

| Путь | Что внутри |
|---|---|
| `repo/src/evaluation/` | модули пайплайна (schema linking, retrieval, planner, candidate generator/selector, dialect-нормализаторы, валидаторы) и версии бейзлайнов B0–B7 |
| `tools/`, `tools/remote_scripts/` | запуск экспериментов: локальные раннеры и скрипты, исполняемые на GPU-рантайме |
| `spider2_dbt_bridge/`, `colab_bridge/`, `snowflake_setup/`, `configs/` | окружение для Spider2-DBT, BigQuery и Snowflake |
| `outputs/REPORT_*.md` | отчёты по фазам |
| `outputs/final/` | материалы ВКР по разделам: бенчмарки, архитектура, прогрессия экспериментов, метрики, анализ результатов |
| `outputs/metrics/`, `outputs/tables/`, `outputs/plots/` | метрики, сводные таблицы и графики |
