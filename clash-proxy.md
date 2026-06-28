---
name: clash-proxy
description: Toggle git HTTP proxy through Clash for GitHub access. Use when git push/pull/clone fails with network errors.
---

# Clash Proxy for Git

Toggle git proxy settings using the local Clash proxy.

> **Setup:** Replace `CLASH_PROXY_PORT` with your Clash mixed-port (default 7890).
> For REST API features, set `CLASH_SECRET` from your Clash config.yaml `secret` field.

## When to Use

- `git push/pull/clone` fails with "Connection was reset" or "Couldn't connect to server"
- User says "开启代理" / "打开代理" / "enable proxy"
- User says "关闭代理" / "disable proxy"

## The Commands

### Enable proxy

```bash
git config --global http.proxy http://127.0.0.1:CLASH_PROXY_PORT
git config --global https.proxy http://127.0.0.1:CLASH_PROXY_PORT
```

### Disable proxy

```bash
git config --global --unset http.proxy
git config --global --unset https.proxy
```

### Check status

```bash
git config --global http.proxy 2>/dev/null && echo "Proxy ON" || echo "Proxy OFF"
```

### Verify Clash is running

```bash
curl -s -o /dev/null -w "%{http_code}" http://127.0.0.1:CLASH_PROXY_PORT --connect-timeout 3
# Returns 400 = Clash proxy working
# Timeout = Clash not running
```

## Clash REST API (Optional)

Find your controller port and secret in `~/.config/clash/config.yaml`:
```yaml
external-controller: 127.0.0.1:PORT
secret: YOUR_SECRET
```

```bash
SECRET="YOUR_CLASH_SECRET"
BASE="http://127.0.0.1:CONTROLLER_PORT"

# Health check
curl -s -H "Authorization: Bearer $SECRET" $BASE

# List all proxies
curl -s -H "Authorization: Bearer $SECRET" $BASE/proxies

# Set proxy mode (Rule/Global/Direct)
curl -s -X PATCH -H "Authorization: Bearer $SECRET" -H "Content-Type: application/json" \
  $BASE/configs -d '{"mode":"Rule"}'
```

## Process

1. Run "check status" to see current proxy state
2. Run "verify Clash is running" — if Clash is not running, tell user
3. Enable/disable proxy as requested
4. After enabling, retry the git operation that failed

## Red Flags

- Never blindly enable proxy without checking Clash is running first
- Always check status before toggling
- If Clash is not running, tell the user to start Clash first
- Do NOT change Clash mode unless explicitly asked
