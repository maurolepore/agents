# agents

Agent-specific tools and config. Human-run scripts live in `~/git/bin`;
anything here is meant to be invoked by coding agents (token-cheap,
non-interactive, full output to files).

## Layout

- `bin/` — executables for agents (on `PATH` via setup below).
- `skills/` — agent skills (opencode-compatible).
- `commands/` — slash-command definitions.
- `settings/` — shared agent settings / defaults.

## Setup (each machine)

```sh
git clone <remote> ~/git/agents
export PATH="$HOME/git/agents/bin:$PATH"
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
