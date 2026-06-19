# Ollama Cloud Models — Ensemble-Ready

## Discovered 17 June 2026

Ollama cloud models are accessible through the local Ollama API (port 11434)
using the `:cloud` suffix. Requires signed-in account on ollama.com.

## Как вызывать

```bash
curl -X POST http://localhost:11434/api/generate \
  -d '{"model":"minimax-m3:cloud","prompt":"...","stream":false}'
```

## Проверенные модели

| Модель | Время | Статус |
|--------|-------|--------|
| `minimax-m3:cloud` | 1.06s | ✅ агент (1M ctx, кодинг) |
| `nemotron-3-ultra:cloud` | 0.34s | ✅ арбитр (550B) |
| `qwen3-coder-next:cloud` | 0.23s | ✅ агент (кодинг) |
| `nemotron-3-super:cloud` | 1.92s | ⚠️ пустой ответ |
| `deepseek-v4-flash:cloud` | — | ❌ требует подписку |
| `kimi-k2.7-code:cloud` | — | ❌ не отвечает |

## Подключение к Hermes

```yaml
# config.yaml — custom_providers
custom_providers:
  - name: ollama-cloud
    base_url: http://localhost:11434
    api_key: ""
    models:
      minimax-m3:cloud:
        context_length: 1048576
      nemotron-3-ultra:cloud:
        context_length: 1000000
      qwen3-coder-next:cloud:
        context_length: 262144
```
