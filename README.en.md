# Inline · Understand where you read

A single-file reading tool. Select text, a Q&A panel opens right there. Follow-ups stack into a chain. When you're done, collapse them: the article is an article again, with a few faint underlined notes left behind.

One `Inline.html`. No dependencies. Open it and read.

<p align="center">
  <a href="https://itzsu123.github.io/Inline/Inline.html"><img src="https://github.com/itzsu123/Inline/raw/main/assets/logo.svg" alt="Inline" width="330"></a>
</p>

<p align="center">
  <a href="https://itzsu123.github.io/Inline/Inline.html"><img src="https://github.com/itzsu123/Inline/raw/main/assets/tagline-en.svg" alt="The answer should grow where the question is — not somewhere else." width="760"></a>
</p>

<p align="center"><sub><a href="README.md">中文</a> · <b>English</b></sub></p>

<p align="center">
  <a href="https://github.com/itzsu123/Inline/releases/latest/download/Inline.html">
    <img alt="Download Inline.html" src="https://img.shields.io/badge/download-Inline.html-2563eb?style=for-the-badge">
  </a>
  &nbsp;
  <a href="https://itzsu123.github.io/Inline/Inline.html">
    <img alt="Open on your phone" src="https://img.shields.io/badge/phone-open%20it-6d5bd0?style=for-the-badge">
  </a>
</p>

<p align="center"><sub>Desktop: download and double-click &nbsp;·&nbsp; Phone: use the right button, no download, storage stays tied to the domain</sub></p>

<p align="center"><sub>Needs a DeepSeek API key — enter it once; it stays in your browser</sub></p>

<p align="center">
  <img src="assets/demo-2.png" width="620" alt="Select a word and the answer lands under that sentence">
</p>

<p align="center"><sub>Select a word: the answer lands under that sentence — not beside it, not in a floating window.</sub></p>

<p align="center">
  <img src="assets/demo-3.png" width="620" alt="Select a phrase inside an answer and the follow-up nests">
</p>

<p align="center"><sub>Select a phrase inside an answer and the follow-up nests one level deeper.</sub></p>

<p align="center">
  <img src="assets/demo-4.png" width="620" alt="Collapsed: one faint dotted line left in the text">
</p>

<p align="center"><sub>Collapse it and the article is an article again — one faint dotted line left behind.</sub></p>

---

## What it's for

When you want to ask something mid-read, you have two options today:

- **Leave and search.** You come back and have to find the sentence again. Attention dropped once.
- **Open a sidebar or a floating panel.** The answer lives in a different plane, and you keep looking back and forth between two places.

Both ask you to do the same thing: **carry your attention away from the text, then carry it back.**

Inline makes a simpler assumption: the answer belongs wherever your attention already is.

## Where your data lives

**In your browser, and nowhere else.**

- Article and every panel are stored in `localStorage`. Refresh and they're still there.
- Your API key lives in `localStorage` too. Requests go straight from the browser to the API; there is no server in between.
- **Export** writes the whole article plus its Q&A to Markdown.
- **Backup / Import** moves everything to another browser via JSON.

No account, no backend, no analytics.

## Swapping the model

The model isn't hardcoded in the logic. There are two constants at the top of the file:

```js
const API_BASE  = "https://api.deepseek.com";
const API_MODEL = "deepseek-chat";
```

**Any service exposing an OpenAI-compatible `/chat/completions` endpoint works.** For example:

| Service | API_BASE | API_MODEL |
|---|---|---|
| DeepSeek (default) | `https://api.deepseek.com` | `deepseek-chat` |
| Qwen (DashScope) | `https://dashscope.aliyuncs.com/compatible-mode/v1` | `qwen-plus` |
| Kimi (Moonshot) | `https://api.moonshot.cn/v1` | `moonshot-v1-8k` |
| Local ollama | `http://localhost:11434/v1` | `qwen2.5:7b` ¹ |

¹ **Local ollama must allow the browser first**, otherwise the request is blocked by CORS (you'll see `Failed to fetch`, which looks like a network problem):

```bash
# macOS / Linux
OLLAMA_ORIGINS=* ollama serve

# Windows
set OLLAMA_ORIGINS=*
ollama serve
```

`*` lets any web page call your local model. Fine for personal use; shut it down afterwards.

After switching, the key field should hold **that provider's** key.

Two known edges:

- Only the plain `chat/completions` is used — no provider-specific parameters.
- A wrong model name is surfaced verbatim from the server inside the panel (with a retry).

## Why it is a single file

**Not a style choice — it follows from the entry point: this tool is meant to be downloaded, double-clicked and opened, not installed, configured and run. One extra step in that path and it stops working.**

Three things follow at once:

- **No infrastructure to distribute.** Nothing to deploy, no domain to renew, no service that goes down. Sending it to someone is sending one file.
- **"Your data stays in your browser" is a structural fact, not a promise.** With no backend there is nowhere to upload it silently; the file is small enough to read through yourself.
- **Its lifetime depends on no one.** No dependencies means nothing to go stale. The file you send today still behaves the same three years from now.

The price is real: **every capability has to be written here** — range anchoring, offset serialization, markdown conversion, image pasting, persistence.

So the test for pulling anything in is two questions:

1. If it breaks, can I fix it?
2. Could I write this more correctly than it is written?

**Two yeses: write it. Either no: depend on it.**

And in some areas (crypto, time zones, PDF parsing) writing it myself would certainly be wrong — so we simply do not do them. **Not doing it is the only dependency that never needs maintenance.**

## What this deliberately doesn't do

No accounts. No cloud sync. No history. No mobile app. No backend.

No PDFs either — this is inline Q&A, and there are far cleverer ways to go about it.

## License

MIT
