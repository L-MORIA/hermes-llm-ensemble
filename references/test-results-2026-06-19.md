# Тест llm-ensemble — 19 июня 2026

> Задача: is_palindrome (Python)
> Провайдер: OpenCode Zen (бесплатно)
> OpenCode CLI v1.17.8

## Режим: Судья (3 агента через CLI + арбитр)

### Агенты (параллельно)

| Агент | Модель | Размер | Время | Решение |
|-------|--------|--------|-------|---------|
| A | DeepSeek V4 Flash | 284B | ~6s | `isalnum()`, чистый Python, без импортов |
| B | MiMo V2.5 | 7B | ~15s | `regex`, только ASCII, создал файл |
| C | Big Pickle | 70B | ~10s | `string.whitespace+punctuation`, verbose |

### Арбитр

| Модель | Время | Вердикт | Статус |
|--------|-------|---------|--------|
| Nemotron-3 Ultra (550B) | ~20s | **Победил A** — isalnum, без ReDoS, без импортов | ✅ краткий промпт |
| Nemotron-3 Ultra (550B) | ❌ Error | Длинный промпт (200+ слов) — упал | ❌ длинный промпт |

### Сравнение через delegate_task (Hermes subagents)

| Агент | Модель | Время | Примечание |
|-------|--------|-------|------------|
| A | DeepSeek V4 Flash | 9s | Все 3 агента используют ОДНУ модель Hermes |
| B | DeepSeek V4 Flash | 8s | ❌ Нет diversity моделей |
| C | DeepSeek V4 Flash | 12s | delegate_task не меняет модель для subagent'ов |

## Выводы

1. **Diversity — только через CLI**: `delegate_task` не даёт разных моделей. Нужен `opencode run --model <model>` для каждого агента
2. **Ultra нестабилен**: на длинных промптах >200 слов может упасть. На коротких/средних — работает
3. **Fallback**: если Ultra упал — DeepSeek V4 Flash (284B) как замена
4. **OpenCode CLI**: `npm i -g opencode-ai@latest` — установить для diversity
|5. **Время**: ~90s (агенты) + 20-60s (арбитр) = ~2-3 мин
|   - В слоте #2 (rate limiter): агенты ~15-24s + арбитр 98s = ~2.5 мин

## Слот #2 — Sliding Window Log Rate Limiter (сложная)

### Агенты (параллельно, OpenCode CLI)

| Агент | Модель | Размер | Время | Особенности |
|-------|--------|--------|-------|-------------|
| A | DeepSeek V4 Flash | 284B | **15s** | `deque`, `threading.Lock`, `time.monotonic()`, `get_remaining` |
| B | MiMo V2.5 | 7B | **21s** | `defaultdict(list)`, `threading.Lock`, `time.monotonic()` |
| C | Big Pickle | 70B | **24s** | `deque`, `threading.Lock`, `time.monotonic()` |

### Арбитр

| Модель | Время | Вердикт | Статус |
|--------|-------|---------|--------|
| Nemotron-3 Ultra (550B) | **98s** | **A** — deque O(1), get_remaining, self-contained | ✅ работал (промпт <150 слов) |
| DeepSeek V4 Flash (284B, fallback) | ~8s | **A** — time.monotonic() | ✅ резерв |

### Сводное сравнение с ensemble-ollama-cloud (та же задача)

| Параметр | 🟢 ensemble-ollama-cloud (Ollama Cloud) | 🔵 llm-ensemble (OpenCode Zen) | 🏆 |
|----------|----------------------------------------|-------------------------------|----|
| Agent A | minimax-m3: **147s** / 5 471 tok | DeepSeek: **15s** | 🔵 в **10x** быстрее |
| Agent B | qwen3-coder-next: **12s** / 1 220 tok | MiMo: **21s** | 🟢 быстрее, но токенов нет |
| Agent C | — | Big Pickle: **24s** | ✅ 3-я модель |
| Арбитр | nemotron-3-ultra: **172s** / 1 446 tok | nemotron-3-ultra: **98s** | 🔵 в **1.8x** быстрее |
| Итого | **~5.5 мин** | **~2.5 мин** | 🔵 в **2x** быстрее |
| Качество | minimax-m3 с `monotonic`, qwen3 c `time.time` | **все 3 с `monotonic`** | ✅ llm-ensemble |

### Выводы (слот #2)

1. **OpenCode Zen быстрее**: агенты в 10x+ быстрее Ollama Cloud
2. **Ultra стабилен на среднем промпте**: 98s, выбрал A за deque O(1) + get_remaining
3. **Все модели грамотные**: 3 из 3 используют `time.monotonic()` — vs ensemble-ollama-cloud где qwen3 ошибся с `time.time()`
4. **Ограничение**: в решении C Big Pickle не указал `get_remaining`, решение A (DeepSeek) — самое полное
