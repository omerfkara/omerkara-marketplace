# Ömer Kara Marketplace

**Claude Code plugins and MCP servers for SDLC automation and task management.**

Two production-ready tools:
- 🎯 **omerkara-sdlc** — Skill for project initialization
- 🔗 **task-mcp** — MCP server for task management API

---

## 🚀 Installation

### Option 1: Add Entire Marketplace (Recommended)

In Claude Code, run:
```
/plugin marketplace add omerfkara/omerkara-marketplace
```

Then install individual plugins:
```
/plugin install omerkara-sdlc
/plugin install task-mcp
```

### Option 2: Install Individual Plugins

**omerkara-sdlc (Skill)**:
```
/plugin marketplace add omerfkara/omerkara-sdlc
/plugin install omerkara-sdlc
```

**task-mcp (MCP Server)**:
```
/plugin marketplace add omerfkara/task-mcp
/plugin install task-mcp
```

---

## 📦 What's Included

### omerkara-sdlc — Project Initialization Skill

**Trigger**: `/omerkara-sdlc` (in Claude Code)

**Features**:
- ✅ Fetch tasks from n8n API
- ✅ Load project PROMPT.md and SCOPE.md
- ✅ Auto-generate missing documentation
- ✅ Interactive project setup

**Requirements**:
- `TASK_TOKEN` — Get from [tasks.omerkara.com/login](https://tasks.omerkara.com/login)

**Usage**:
```
/omerkara-sdlc
→ Loads tasks
→ Loads documentation
→ Creates templates if needed
→ Ready to work!
```

**Repository**: https://github.com/omerfkara/omerkara-sdlc

---

### task-mcp — MCP Server for Task Management

**Type**: Model Context Protocol Server (works with Claude Code, Claude API, or standalone)

**Tools** (8 total):
- `task_list_tasks` — Get project tasks
- `task_create_task` — Create new task
- `task_update_task` — Update task
- `task_delete_task` — Delete task
- `task_list_documents` — Get documents
- `task_upsert_document` — Create/update document
- `task_delete_document` — Delete document

**Requirements**:
- Python 3.8+
- `pip install mcp pydantic httpx`
- `TASK_TOKEN` — Get from [tasks.omerkara.com/login](https://tasks.omerkara.com/login)

**Usage in Claude Code**:

Add to `.claude/mcp_servers.json`:
```json
{
  "mcpServers": {
    "task_mcp": {
      "command": "python",
      "args": ["/path/to/task-mcp/task_mcp.py"]
    }
  }
}
```

Then ask Claude to use the tools:
```
List all open tasks for rivalsense
→ Uses task_list_tasks tool

Create a high-priority auth task
→ Uses task_create_task tool
```

**Repository**: https://github.com/omerfkara/task-mcp

---

## ⚙️ Configuration

### Get Your Token (One-Time)

1. Go to [tasks.omerkara.com/login](https://tasks.omerkara.com/login)
2. Click "Hesap oluştur" (Create Account)
3. Enter username, email, password
4. Token sent to your email
5. Set in environment:
   ```bash
   export TASK_TOKEN=your-token
   ```

### In Claude Code

Store token in `.env` or shell environment:
```bash
# Terminal
export TASK_TOKEN=your-token

# Or in .env (gitignored)
TASK_TOKEN=your-token
```

### In .claude/mcp_servers.json

```json
{
  "mcpServers": {
    "task_mcp": {
      "command": "python",
      "args": ["/path/to/task-mcp/task_mcp.py"],
      "env": {
        "TASK_API": "https://n8n.omerkara.com/webhook"
      }
    }
  }
}
```

Token is read from system environment.

---

## 📚 Documentation

Each plugin has comprehensive docs:

- **omerkara-sdlc**: https://github.com/omerfkara/omerkara-sdlc
- **task-mcp**: https://github.com/omerfkara/task-mcp
- **API Reference**: https://tasks.omerkara.com/task-management.md

---

## 🎯 Workflow Examples

### New Project Setup

```
1. /omerkara-sdlc
   ↓
2. Creates PROMPT.md, SCOPE.md
   ↓
3. Loads existing tasks
   ↓
4. Ready to develop!
```

### Daily Development

```
1. List open tasks
   task_list_tasks(project="rivalsense")
   
2. Pick a task, start working
   task_update_task(id=42, status="in_progress")
   
3. Discover new work
   task_create_task(project="rivalsense", title="...")
   
4. Mark complete
   task_update_task(id=42, status="done")
```

### Update Documentation

```
task_upsert_document(
  project="rivalsense",
  name="SCOPE",
  content="# Updated scope..."
)
```

---

## 🔒 Security

✅ Tokens never in source code  
✅ `.env` files are `.gitignore`'d  
✅ Per-user token management  
✅ HTTP-only authentication  
✅ Pydantic input validation  

**Security Best Practices**:
1. Store `TASK_TOKEN` in environment only
2. Never commit `.env` to git
3. Rotate tokens periodically
4. Use system `.env` for shared machines

---

## 📊 System Architecture

```
Claude Code / Claude API
       │
   ┌───┴──────────────┐
   │                  │
Skill: /omerkara-sdlc │  MCP: task_mcp
   │                  │
   └───┬──────────────┘
       │
n8n Task API
(n8n.omerkara.com/webhook)
       │
PostgreSQL
(tasks.omerkara.com)
```

---

## 🚢 Deployment Options

### Personal Machine
```bash
# Skill
ln -s ~/code/omerkara-sdlc ~/.claude/skills/

# MCP Server
python ~/code/task-mcp/task_mcp.py
```

### Team Server
```bash
# Skill (shared)
/opt/claude-skills/omerkara-sdlc

# MCP Server (hosted)
systemctl start task-mcp
```

### Docker
```bash
docker run -e TASK_TOKEN=... omerfkara/task-mcp
```

---

## 🐛 Troubleshooting

### Skill not found
```bash
# Check Claude skills directory
ls ~/.claude/skills/omerkara-sdlc/SKILL.md
```

### "TASK_TOKEN not found"
```bash
# Set in environment
export TASK_TOKEN=your-token

# Verify
echo $TASK_TOKEN  # Should not be empty
```

### MCP server won't start
```bash
# Install dependencies
pip install -r task-mcp/requirements.txt

# Test server
python task-mcp/task_mcp.py
```

### "API error 403"
Token is invalid or expired. Get new token from tasks.omerkara.com/login.

---

## 📞 Support

- **API Docs**: [tasks.omerkara.com/task-management.md](https://tasks.omerkara.com/task-management.md)
- **Task Dashboard**: [tasks.omerkara.com](https://tasks.omerkara.com)
- **GitHub Issues**: Open issue on relevant repo
- **Contact**: See repo README files

---

## 📄 License

Both plugins: **MIT License**

---

## 🔗 Links

- **omerkara-sdlc**: https://github.com/omerfkara/omerkara-sdlc
- **task-mcp**: https://github.com/omerfkara/task-mcp
- **Tasks Dashboard**: https://tasks.omerkara.com
- **API Reference**: https://tasks.omerkara.com/task-management.md

---

**Ready to use!** Install and configure, then start with `/omerkara-sdlc` 🚀
