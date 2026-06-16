# AGENTS.md

## Описание для AI-агентов (Claude Code, Codex, Cursor, Copilot)

### Обзор проекта

Hermes LLM Ensemble — скил для Hermes Agent, реализующий ансамбль LLM с арбитром. Позволяет запускать одну задачу на 3 разных моделях параллельно, затем арбитр оценивает и выбирает/синтезирует лучшее решение.

### Ключевые файлы

| Файл | Назначение |
|------|------------|
| `SKILL.md` | Сам скил для Hermes Agent. Устанавливается в `~/.hermes/skills/llm-ensemble/` |
| `README.md` | Инструкция для пользователя |
| `ARCHITECTURE.md` | Архитектура и дизайн |
| `TEST_REPORT.md` | Результаты тестирования |
| `AGENTS.md` | Этот файл |

### Технологии

- **Hermes Agent** — фреймворк для запуска скила
- **OpenCode CLI** — запуск моделей через `opencode run --model <model>`
- **OpenCode Zen** — бесплатный провайдер (не требует API-ключа)
- **Модели:** DeepSeek V4 Flash, Xiaomi MiMo V2.5, Big Pickle, Nemotron-3 Ultra

### Как использовать

```python
# Запуск ensemble:
# 1. Установить скил: cp SKILL.md ~/.hermes/skills/llm-ensemble/
# 2. В сессии Hermes: /skill llm-ensemble
# 3. Дать задачу + режим (судья/синтезатор/мозговой штурм)
```

### Тестирование

Полный отчёт тестов — в `TEST_REPORT.md`. Три режима арбитра протестированы на трёх задачах, измерены diversity и время выполнения.

### Публикация

```bash
# Публикация в Hermes Skills Hub
hermes skills publish ./SKILL.md
```
