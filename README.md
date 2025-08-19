# Local LLM Chat UI (Tkinter + Jupyter)

A polished, privacy-first desktop UI for chatting with **locally hosted LLMs** via an **OpenAI-compatible API** (e.g., **Ollama**). Launchable from Jupyter or as a plain Python script. Markdown rendering, multi-theme styling, file-context ingestion, streaming, and rich export formats included. Built by @benrushscience and GPT-5 thinking.

---

## Highlights

- **Local-first**: talk to models running on your own machine.
- **Markdown chat**: headings, tables, fenced code—rendered with `tkinterweb`.
- **Themes**: Apple Light, Chrome Gray, Matte Dark (fully applied to widgets & menus).
- **Files as context**: load `.txt/.md/.py/.sas/.docx/.rtf/.csv/.xlsx/.pptx/.ipynb` (extensible).
- **Streaming** with Stop and auto-scroll to the latest message.
- **Exports**: Save As ▾ **JSON / MD / TXT / RTF / PDF** (ReportLab).
- **Model picker**: lists local Ollama models; quick Refresh.
- **Token budgeting (optional)** with a Hugging Face tokenizer to fit context windows.
- **Config via `.env`** (keep secrets out of Git).

---

## Prerequisites

- Python 3.9+
- A running OpenAI-compatible endpoint (e.g., **Ollama** at `http://localhost:11434/v1`)

> The UI auto-installs core packages: `openai`, `tkinterweb`, `markdown`. Optional features install on demand (`pandas`, `python-docx`, `striprtf`, `python-pptx`, `reportlab`, etc.).

---

## Install & Run

1. **Clone** this repo and open the notebook/script.
2. **Set environment variables** (recommended; see “Config” below).
3. **Run** the main cell/script. The notebook cell stays busy while the Tk window is open.

### Config (private, GitHub-friendly)

Create a local `.env` (do **not** commit) and set:

OLLAMA_BASE_URL=http://localhost:11434/v1
OLLAMA_API_KEY=ollama

Provide a `.env.example` in the repo; add `.env` to `.gitignore`.

---

## Using the App

- **Base URL / API Key / Model**: top bar; press **Refresh** to re-enumerate local models.
- **System Prompt**: set initial behavior for the assistant.
- **Render Markdown / Apply Theme / Stream**: grouped controls (top right).
- **Files**: “Add files” → select; toggle “Include files in next message”.
- **Max chars per file**: spinner caps per-file text injected into the next prompt.
- **Send** / **Stop** / **Clear**: bottom right.
- **Save As ▾**: export transcript to JSON, MD, TXT, RTF, or PDF.  
- **Load JSON**: restore a prior session.

**Shortcuts**:  
Enter = Send · Shift+Enter = New line

---

## Working with Ollama

- Website & docs: [https://ollama.com](https://ollama.com)
- Install (see platform instructions on the site), then start the server:
  ```bash
  ollama serve
  ollama pull llama3
  ollama pull llama3:70b
  ollama run llama3
  ollama list
  ```
- Tip: Large models may require substantial VRAM; choose a size/quantization that fits your GPU.

## File Ingestion (extensible)

Built-ins: .txt/.md/.py/.sas.
Optional extractors (auto-install on first use): .docx (python-docx), .rtf (striprtf), .csv/.xlsx (pandas), .pptx (python-pptx), .ipynb (native JSON parse).

You can register your own extensions by adding to the EXTRACTORS map:
EXTRACTORS[".ext"] = my_extractor_function

## Token & Length Guidance
- Models enforce a context window (prompt + reply ≤ max tokens).
- Use the optional token budgeting utility to trim older turns and large file blocks.
- Control reply length via max_tokens (OpenAI style) or num_predict (Ollama).

## Troubleshooting
- No models listed: verify ollama serve is running; check OLLAMA_BASE_URL.
- Theme shows white areas: the app includes full ttk/Tk/HtmlFrame theming—ensure you’re on the latest code and click Apply Theme.
- Slow streaming: increase STREAM_UPDATE_N or stream as plain text, Markdown at end.
- Scroll jumps to top: auto-scroll is enabled; ensure the scroll_to_bottom() patch is present.

## Acknowledgments
- Ollama for local model serving.
- tkinterweb for in-window HTML/Markdown rendering.
- ReportLab for PDF export.
