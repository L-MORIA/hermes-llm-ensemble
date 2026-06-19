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
5. **Время**: ~90s (агенты) + 20-60s (арбитр) = ~2-3 мин
