---
description: Toggle oh-my-openagent on/off
---
Toggle oh-my-openagent plugin based on $ARGUMENTS. NOTE: `opencode.json` is JSONC (contains `//` comments) — never use `jq`, it fails to parse. Use Read + Edit tools, `cp` for backup, `grep` for state checks.
- If `$ARGUMENTS` is `on`: Read `~/.config/opencode/opencode.json`. If no `"plugin"` line, backup via `cp opencode.json opencode.json.bak-$(date +%Y%m%d-%H%M%S)`, then Edit to insert `  "plugin": ["oh-my-openagent@latest"],` after the `"$schema"` line. Keep `~/.omo/omo.jsonc` as-is.
- If `$ARGUMENTS` is `off`: Read `~/.config/opencode/opencode.json`. Backup via `cp`, then Edit to remove the `  "plugin": ["oh-my-openagent@latest"],` line. Leave everything else untouched.
- For any other `$ARGUMENTS`: show current state (`grep -n plugin ~/.config/opencode/opencode.json || echo "plugin absent (vanilla)"` and `ls ~/.omo/omo.jsonc`) and usage `/omo on|off`.
After editing, tell user to `/exit` and restart `opencode` — plugin changes require restart, no hot-reload.
