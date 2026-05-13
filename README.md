# WriteRight ✍️

A Grammarly-style writing assistant that runs entirely in your browser and uses your local [Ollama](https://ollama.com) instance for AI — no cloud, no API keys, no backend.

Vibe coded in ~1 hour using Claude (claude.ai).

---

## What it does

- Grammar, spelling, style, and suggestion checks via any Ollama model
- Inline underline highlights — click to accept or ignore fixes
- Learns from your accepted corrections and feeds them back into future checks
- Tracks your writing profile (sentence length, vocabulary richness, passive voice, tone)
- Manage multiple Ollama models and switch between them on the fly
- Auto-check mode that triggers after you stop typing

---

## Requirements

- [Ollama](https://ollama.com) installed and running locally
- At least one model pulled, e.g. `ollama pull llama3`
- A modern browser (Chrome, Firefox, Edge)

---

## Setup

Start Ollama with CORS enabled — this is required for the browser to talk to it:

```bash
# macOS / Linux
OLLAMA_ORIGINS="*" ollama serve

# Windows (PowerShell)
$env:OLLAMA_ORIGINS="*"; ollama serve
```

Then just open `WriteRight.html` in your browser. No install, no build step.

---

## Adding models

Click **⚙ Models** in the top bar. You can:

- Hit **↓ Fetch from Ollama** to auto-import all your installed models
- Manually type a model name (e.g. `mistral`, `phi4`, `deepseek-r1:7b`) and add it
- Set the active model from the toolbar dropdown before checking

Models that work well for this task: `llama3`, `mistral`, `phi3`, `gemma2`, `deepseek-r1`.

---

## Stack

Everything is in a single `.html` file. No framework, no build tool, no dependencies to install.

| Thing | What it does |
|---|---|
| Vanilla JS | All logic — highlight rendering, offset tracking, style learning |
| Ollama `/api/chat` | Local LLM inference |
| `localStorage` | Persists models, accepted corrections, writing profile |
| Google Fonts | Lora (editor), Instrument Sans (UI), DM Mono (code/stats) |
| CSS custom properties | Theming, dark mode |

The highlight overlay works by layering a transparent `<textarea>` over a `<div>` that mirrors the text with `<mark>` tags injected at exact character offsets. When a fix is accepted, all subsequent issue offsets are adjusted by the character delta.

---

## Limitations

- Works best with instruction-tuned models. Base models may not return clean JSON reliably.
- The HTML file must be served from a context that allows `fetch()` to `localhost` — opening it directly from disk (`file://`) may be blocked by some browsers. Serve it with any static server, e.g. `npx serve .`
- No authentication, so don't expose your Ollama port publicly.

---

## License

MIT — do whatever you want with it.
