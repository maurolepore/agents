# AGENTS.md

Meta-repo for agent-invoked tooling. Human-run scripts live in `~/git/bin`; this repo is only for token-cheap, non-interactive wrappers.

## Layout

- `bin/` — executables (on `PATH` via `~/git/agents/bin`). Only directory with real code.
- `skills/` — `SKILL.md` per subdir; canonical store (empty repo ships with 4 merged skills). Discovered globally via symlinks (see Setup).
- `commands/` — `*.md` per command (ships with `omo.md`). Ditto.
- `settings/` — repo-local only (no opencode discovery path — do not symlink globally).
- `.opencode/` — `skills -> ../skills`, `commands -> ../commands` symlinks for project-local discovery (portable fallback; global symlinks are primary).

## Setup

```sh
export PATH="$HOME/git/agents/bin:$PATH"  # required — wrappers not found otherwise
setup-agents                              # one-time per machine: ensures PATH in ~/git/dotfiles/zshrc + calls setup-opencode
setup-opencode                            # ...or opencode-only: creates global symlinks so opencode discovers skills/commands from any cwd
```

Requires R packages `rcmdcheck`, `devtools`, `covr` in the R library the wrappers invoke via `Rscript --vanilla`.

Global discovery per https://opencode.ai/docs/skills/ + https://opencode.ai/docs/commands/:
`~/.config/opencode/skills`, `~/.agents/skills`, `~/.claude/skills` -> `~/git/agents/skills`;
`~/.config/opencode/commands` -> `~/git/agents/commands`. `setup-opencode` merges any pre-existing global dirs before linking (idempotent, `--dry-run` available). `setup-agents` additionally ensures `export PATH="$HOME/git/agents/bin:$PATH"` in `~/git/dotfiles/zshrc` for persistence. Restart shell/opencode after running.

## Wrappers (token-cheap)

All wrappers print a summary to stdout only; full log + parsed `.rds` go next to the log file. On success stop — never `cat`/`tail` the log. Drill in only on failure via `*-summary <logfile>`.

```sh
rcmd-check [pkgdir] [--log PATH]          # R CMD check: Status + failing checks only (args: --no-manual)
rtest [pkgdir] [--filter REGEX] [--log PATH]  # testthat: counts + failing tests only
rcoverage [pkgdir] [--log PATH]            # coverage: total + files <100%; meaningless if tests fail — run rtest first

rcmd-check-summary [logfile]  # re-print summary (defaults to most recent /tmp/rcmd-check-*.log)
rtest-summary [logfile]       # defaults to most recent /tmp/rtest-*.log
rcoverage-summary [logfile]   # defaults to most recent /tmp/rcoverage-*.log
```

Defaults: log to `/tmp/<tool>-<pkg>-<timestamp>.log`, `.rds` sibling at same path with `.rds` extension. Pass values via env vars to R — no shell interpolation.

Never use `devtools::test()`, `devtools::check()`, or `covr::package_coverage()` directly in agent sessions; use wrappers above. For single-file fast feedback (small output) use `devtools::test_active_file()` / `devtools::load_all()` directly.

## `update-tidy-agents`

Restores the `### Key commands` block in downstream packages after `usethis::use_tidy_agents()`:

```sh
update-tidy-agents [pkgdir]  # rewrites AGENTS.md Key commands; idempotent, aborts if anchors (devtools::test()/check()) missing
```

Requires the managed marker `<!-- update-tidy-agents: managed block, do not edit by hand -->` and the `### Key commands` / next `### ` boundaries.

## Conventions

- Shell scripts use `set -u`; parse args manually (`--log`, `--filter`, `-h/--help`).
- No build system, lint config, CI, or `opencode.json` in this repo — global config at `~/.config/opencode/opencode.json` applies.
- Verify wrapper edits with `bash -n bin/<script>` and a dry run (`<tool> --help`); this repo has no test suite of its own.

## Public repo gate

This repo will be public. Never add secrets, tokens, private keys, `.env`/`Renviron`, personal paths, email addresses beyond git author, or dotfiles content. Keep `~/git/bin` (private, human-run) vs `~/git/agents/bin` (public, agent-run) separate. Before adding anything user-specific (Mauro-specific aliases, workflows, machine-specific paths, or assumptions that only fit one user), use the `question` tool to ask for confirmation — the repo should stay useful for others.
