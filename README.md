# research-mode

Anti-hallucination research mode for Claude Code. One command toggles 4 constraints that fundamentally change output quality.

Based on [Anthropic's official docs](https://docs.anthropic.com/en/docs/test-and-evaluate/strengthen-guardrails/reduce-hallucinations) — specifically the advanced techniques that most people skip.

## What it does

Type `/research` and Claude stops guessing.

| Constraint | What it does | Why |
|-----------|-------------|-----|
| **Verify with citations** | Every claim needs a source. No source = claim gets retracted | Removes confident-sounding fiction |
| **Direct quote grounding** | Extracts exact quotes from docs before analyzing | Stops paraphrase-drift where meaning subtly changes |
| **Chain-of-thought verification** | Shows reasoning step by step before concluding | Surfaces faulty logic and bad assumptions |
| **External knowledge restriction** | Only uses info from provided documents | You know exactly what came from your source vs. training data |

Exit with "exit research mode" — Claude goes back to normal.

## Install

**As a plugin (recommended):**

```
claude plugin add dogwiz/research-mode
```

**As a skill:**

Clone this repo into your `.claude/skills/` directory:

```bash
git clone https://github.com/dogwiz/research-mode.git ~/.claude/skills/research-mode
```

## Usage

```
/research
```

Or with a topic:

```
/research what caused the Change Healthcare breach
```

## What about the 3 "basic" techniques?

Anthropic's docs list 3 basic and 4 advanced techniques. The basic 3 are:

1. Allow Claude to say "I don't know"
2. Verify with citations
3. Use direct quotes for factual grounding

We think **#1 should be on all the time** — not just in research mode. Claude should never fill knowledge gaps with fiction regardless of context.

This plugin focuses on the 4 techniques that make sense as a **toggle**: citation requirements, quote grounding, chain-of-thought verification, and external knowledge restriction. These add rigor that's valuable for research but would slow down creative work.

If you want #1 always-on, add this to your `~/.claude/rules/` or `CLAUDE.md`:

```
If you don't have a credible basis for a claim, say so.
Never fill knowledge gaps with plausible fiction.
"I don't have enough information" is always a valid answer.
```

## How it works

This is a single markdown file (`commands/research.md`) wrapped in a Claude Code plugin. No dependencies, no build step, no API calls. Claude reads the constraints and follows them until you exit.

The constraints are based on [Anthropic's reduce-hallucinations documentation](https://docs.anthropic.com/en/docs/test-and-evaluate/strengthen-guardrails/reduce-hallucinations).

## License

MIT
