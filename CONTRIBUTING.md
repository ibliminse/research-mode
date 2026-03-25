# Contributing

PRs welcome. This is a simple project — here's how to contribute:

## Adding constraints

Edit `commands/research.md`. Each constraint follows the same format:

```markdown
## Constraint N: Name

[What Claude should do]

Why this matters: [one paragraph]
```

Keep it short. Claude reads the entire file every time `/research` is invoked.

## Guidelines

- No dependencies. This is a single markdown file — keep it that way.
- No vague rules. Every constraint should produce a visible change in output.
- Test before submitting. Run `/research` with your change and show the before/after.
- Keep the README in sync with `commands/research.md`.

## Reporting issues

Open an issue with:
1. What you asked Claude to research
2. What went wrong (hallucination, missed citation, etc.)
3. Whether `/research` was active

## License

By contributing, you agree your contributions are MIT licensed.
