# 🌉 Remote Bridge System v5.0 — Multi-Agent Orchestration

A powerful remote terminal bridge designed for AI agents. Run multiple autonomous AI agents (e.g., from z.ai tabs) against a single bridge with concurrency control, mission workspaces, and smart polling.

## ✨ Features

- **Multi-Agent Support** — `agent_id` per request, color-coded terminal logs, concurrency limiter
- **Background Processes** — Run heavy commands in background, poll with smart hints (`poll_after_seconds`)
- **Mission System** — Auto-create structured workspaces per target with signal-based coordination
- **13 Actions** — `exec`, `bg`, `poll`, `list`, `stat`, `read`, `write`, `mkdir`, `delete`, `move`, `upload`, `download`, `stats`, `mission`
- **AI Auto-Discovery** — HTML landing page teaches AI how to use curl when it browses the URL
- **Stability** — Port auto-fallback, gzip compression, session logging

## 🚀 Quick Start

### Option 1: All-in-One Launcher
```bash
python launch.py --api-key your-secret
```
Starts bridge + Cloudflare tunnel automatically.

### Option 2: Manual
```bash
python bridge_agent.py --port 8765 --max-concurrent 2
cloudflared tunnel --url http://localhost:8765 --protocol http2
```

## 🤖 Multi-Agent Workflow

### 1. Create a Mission
```json
{"action": "mission", "target": "example.com"}
```
Creates: `missions/example.com/{recon,endpoints,vulns,reports,signals,loot}/`

### 2. Each AI Agent Uses `agent_id`
```json
{"action": "exec", "command": "nmap -sV target.com", "agent_id": "recon"}
{"action": "bg", "command": "ffuf -u http://...", "agent_id": "fuzzer"}
```

### 3. Terminal Output (Color-Coded)
```
[10:08:01] [recon]   EXEC  nmap -sV target.com
[10:08:03] [fuzzer]  BG    ffuf -u http://...
[10:08:05] [vulns]   READ  /missions/example.com/endpoints/api.json
```

### 4. Smart Poll Hints
```json
// bg response
{"job_id": "a1b2c3d4", "status": "queued", "poll_after_seconds": 10,
 "hint": "2 processes running, 1 queued. Poll again in 10-30 seconds."}

// poll response (still running)
{"status": "running", "elapsed": 15.2, "poll_after_seconds": 10,
 "hint": "Command is running. Poll again in 10-20 seconds."}

// poll response (done)
{"status": "done", "poll_after_seconds": 0, "stdout": "...results..."}
```

### 5. Signal Coordination
Agents coordinate via filesystem flags:
```json
// Recon agent: "I'm done"
{"action": "write", "path": "missions/example.com/signals/recon_done.flag", "content": "done"}

// Fuzzer agent: "Is recon done?"
{"action": "stat", "path": "missions/example.com/signals/recon_done.flag"}
```

## 🔧 CLI Options

| Flag | Default | Description |
|---|---|---|
| `--port, -p` | 8765 | Server port |
| `--api-key, -k` | None | API key for auth |
| `--max-concurrent, -c` | 2 | Max simultaneous bg processes |
| `--missions-dir` | `./missions` | Mission workspace directory |
| `--log, -l` | None | Session log file (.jsonl) |
| `--no-color` | false | Disable colored output |

## 🔒 Security
- **API Key** — Use `--api-key` to protect your bridge
- **HTTPS** — Cloudflare Tunnels provide end-to-end encryption

## 📜 License
MIT
