# 🌉 Remote Bridge System v4.0

A powerful, lightweight, and stable system for remote command execution and filesystem management over HTTP. Specifically designed for AI agents (like Claude, GPT-4, Gemini) to interact with your local machine securely via Cloudflare Tunnels.

## ✨ New in v4.0

- **All-in-One Launcher** - Start both the agent and Cloudflare tunnel with a single command.
- **Background Processes** - Run long-running tasks (like `npm run dev`) and poll for output.
- **Enhanced Filesystem Ops** - Direct `read`/`write` (text), `mkdir`, `move`, `delete`, and `stat` (md5/size).
- **Auto-Discovery** - Rich HTML landing page and JSON metadata for AI agents.
- **Stability** - Automatic port fallback and protocol negotiation (`http2`/`h2mux`).

## 🚀 Quick Start

### 1. Requirements
- Python 3.7+
- `cloudflared` (optional, for remote access)

### 2. Launch Everything
The easiest way to start is using the new launcher:
```bash
python launch.py --api-key your-secret-key
```
This will:
1. Start the **Bridge Agent** on port 8765 (or next available).
2. Start a **Cloudflare Tunnel** automatically.
3. Provide a **URL** you can give to your AI agent.

### 3. Usage for AI Agents
Just give your AI agent the Tunnel URL. If they open it in a browser, they'll see a complete guide on how to use `curl` to interact with your machine.

---

## 🛠️ Available Actions

| Action | Description |
|---|---|
| `exec` | Run synchronous shell commands. |
| `bg` | Run background processes (returns `pid`). |
| `poll` | Check output/status of background processes. |
| `list` | List directory contents (structured JSON). |
| `stat` | Get file metadata (size, md5, mtime). |
| `read` | Read text files directly. |
| `write` | Write/Append to text files. |
| `upload` | Binary file upload (Base64). |
| `download` | Binary file download (Chunked/Base64). |

---

## 🔒 Security
- **API Key** - Use `--api-key` to protect your bridge.
- **HTTPS** - Cloudflare Tunnels provide end-to-end encryption.
- **ReadOnly** (Coming soon) - Option to restrict to non-destructive actions.

## 📜 License
MIT
