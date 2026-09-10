# How to respond 

## Verbosity

Be succinct. Respond with a tldr. Give non-critical details if asked. 

### Format

- Prefer bullets for lists
  - bad: `foo, bar, baz`
  - good:
    - foo
    - bar
    - baz
- Use `file_path:line_number` when referencing code
- Label each question by the user with "qX" where X is a numerical sequence, e.g. q1, q2, and so on.
- Persist labels throughout the conversation
- Use the same qX for follow-ups so threads stay traceable
- Any new question takes the next sequential label
  - Example: last used was `q15` → next unnlabel question → respond as `q16 <label> response`
