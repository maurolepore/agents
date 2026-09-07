# agents

Agent-specific tools and config. Human-run scripts live in `~/git/bin`;
anything here is meant to be invoked by coding agents (token-cheap,
non-interactive, full output to files).

## Layout

- `bin/` — executables for agents (on `PATH` via setup below).
- `skills/` — agent skills (`SKILL.md` per subdir; discovered globally via symlinks, see Setup).
- `commands/` — slash-command definitions (`*.md`; ditto).
- `settings/` — shared agent settings / defaults (repo-local only; no opencode discovery path).
- `.opencode/` — symlinks `skills -> ../skills`, `commands -> ../commands` for project-local discovery (redundant with global but keeps repo portable).

## Setup (each machine)

```sh
git clone <remote> ~/git/agents
export PATH="$HOME/git/agents/bin:$PATH"
setup-agents     # one-time: adds ~/git/agents/bin to PATH in ~/git/dotfiles/zshrc if missing, then calls setup-opencode
setup-opencode   # ...or opencode-only: symlinks ~/.config/opencode/{skills,commands}, ~/.agents/skills, ~/.claude/skills -> ~/git/agents/{skills,commands}; merges existing global content first; idempotent
```

## Tools

All wrappers print summaries only; full log + parsed `.rds` next to the log path.
Log path is printed only on failure -- on success there is nothing to drill into.

- `rcmd-check [pkgdir] [--log PATH]` — R CMD check: Status + failing checks.
- `rtest [pkgdir] [--filter REGEX] [--log PATH]` — tests: counts + failing tests.
- `rcoverage [pkgdir] [--log PATH]` — coverage: total + files below 100%.
- `rcmd-check-summary`, `rtest-summary`, `rcoverage-summary [logfile]` — re-print a summary.
- `update-tidy-agents [pkgdir]` — restore the Key-commands section of `AGENTS.md`
  after `usethis::use_tidy_agents()` (idempotent; aborts if the template drifted).
- `setup-agents [--dry-run]` — general setup: ensure `~/git/agents/bin` on PATH (current shell + `~/git/dotfiles/zshrc` if present) then delegate to `setup-opencode`.
- `setup-opencode [--dry-run]` — wire global discovery: `~/.config/opencode/{skills,commands}`, `~/.agents/skills`, `~/.claude/skills` -> `~/git/agents/{skills,commands}` plus `.opencode/` fallbacks; merges existing dirs first.
