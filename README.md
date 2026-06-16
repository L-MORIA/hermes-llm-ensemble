# Hermes LLM Ensemble

**Одна задача → 3 независимые LLM → арбитр выбирает или синтезирует лучшее решение.**

Скил для [Hermes Agent](https://hermes-agent.nousresearch.com), реализующий паттерн **Mixture of Agents (MoA)** — ансамбль моделей с арбитром. Бесплатно, через OpenCode Zen.

## Возможности

- 🚀 **3 агента параллельно** — DeepSeek, Xiaomi MiMo V2.5, Big Pickle
- 🏆 **Арбитр Nemotron-3 Ultra** — 550B, 1M контекст
- ⚖️ **Судья** — выбирает лучшее решение (код, фикс бага)
- 🧩 **Синтезатор** — собирает лучшее из каждого (архитектура, тесты)
- 💡 **Мозговой штурм** — рождает новое из идей (стратегия, дизайн)
- ⏱ **~3-4 минуты** на задачу

## Быстрый старт

### 1. Установка скила

```bash
# Через Hermes CLI
hermes skills install ./SKILL.md

# Или через профиль (рекомендуется)
cp SKILL.md ~/AppData/Local/hermes/profiles/old-laptop/skills/llm-ensemble/SKILL.md
```

### 2. Загрузка в сессию

```bash
/skill llm-ensemble
```

### 3. Запуск

```
Задача: "Напиши парсер CSV на Python, режим: судья"
→ Agent A (DeepSeek): решение
→ Agent B (MiMo V2.5): решение
→ Agent C (Big Pickle): решение
→ 🏆 Арбитр: выбрано решение A — безопаснее всех
```

## Режимы арбитра

| Режим | Девиз | Для |
|-------|-------|-----|
| ⚖️ **Судья** | «Выбрать лучший» | Код, фикс бага |
| 🧩 **Синтезатор** | «Собрать лучшее из каждого» | Архитектура, рефакторинг, тесты |
| 💡 **Мозговой штурм** | «Родить новое» | Стратегия, дизайн, планирование |

## Модели

Все модели бесплатно через **OpenCode Zen** (провайдер `opencode`):

| Роль | Модель | Характеристика |
|------|--------|----------------|
| 🏆 Арбитр | `opencode/nemotron-3-ultra-free` | NVIDIA 550B, 1M ctx |
| 🤖 Агент A | `opencode/deepseek-v4-flash-free` | DeepSeek V4 Flash |
| 🤖 Агент B | `opencode/mimo-v2.5-free` | Xiaomi MiMo V2.5 |
| 🤖 Агент C | `opencode/big-pickle` | OpenCode Big Pickle |
| 🔄 Резерв | `opencode/north-mini-code-free` | North Mini Code |

## Требования

- [Hermes Agent](https://hermes-agent.nousresearch.com) (установлен)
- [OpenCode CLI](https://opencode.ai) v1.17+ (`npm i -g opencode-ai@latest`)
- OpenCode Zen (бесплатный, работает из коробки)

## Структура проекта

```
hermes-llm-ensemble/
├── SKILL.md              # Сам скил (SKILL.md)
├── README.md             # Эта инструкция
├── ARCHITECTURE.md       # Архитектура и дизайн
├── AGENTS.md             # Описание для AI-агентов
├── TEST_REPORT.md        # Полный отчёт тестирования
├── LICENSE               # MIT
└── .gitignore
```

## Лицензия

MIT
