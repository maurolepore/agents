# global-AGENTS.md — conventions for all projects

Applies to every repo. Project `AGENTS.md` may add project-specific rules on top.

## Semantic prefixes (mlXX) — ml01, ml02, …

- Persist labels `ml01`, `ml02`, … throughout the conversation
- Same `mlXX` for follow-ups so threads stay traceable
- Sequentiality: within a session, any new question with no `mlXX` tag takes the next sequential label
  - Example: last used was `ml15` → next untagged question → respond as `ml16 <slug> — response`

## Style — short + readable

- Minimal without losing clarity
- Prefer bullets for lists
  - bad: `foo, bar, baz`
  - good:
    - foo
    - bar
    - baz
- Use `file_path:line_number` when referencing code
