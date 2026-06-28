# my-claude-skills

Personal collection of Claude Code skills, shared across all projects.

## Skills

| Skill | Description |
|---|---|
| `clash-proxy` | Toggle git HTTP proxy through Clash for GitHub access |

## Installation

```bash
# Clone to global skills directory
git clone https://github.com/lmlit/my-claude-skills.git ~/.claude/skills/my-claude-skills

# Or copy individual skills
cp clash-proxy.md ~/.claude/skills/
```

## Setup

After installation, edit the skill file to replace placeholders with your local values:
- `CLASH_PROXY_PORT` → your Clash mixed-port (default: 7890)
- `YOUR_CLASH_SECRET` → your Clash secret from `~/.config/clash/config.yaml`
- `CONTROLLER_PORT` → your Clash external-controller port
