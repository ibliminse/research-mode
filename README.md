<p align="center">
  <img src="assets/infographic.png" alt="Stop Letting Claude Hallucinate — 3 always-on rules + 4 research-mode constraints" width="700">
</p>

<h1 align="center">research-mode</h1>

<p align="center">
  Anti-hallucination plugin for Claude Code.<br>
  One command. Four constraints. Zero hallucinated "facts" in your research.
</p>

<p align="center">
  <a href="LICENSE"><img src="https://img.shields.io/badge/license-MIT-blue.svg" alt="MIT License"></a>
  <a href="https://docs.anthropic.com/en/docs/test-and-evaluate/strengthen-guardrails/reduce-hallucinations"><img src="https://img.shields.io/badge/based%20on-Anthropic%20Docs-d97757.svg" alt="Based on Anthropic Docs"></a>
  <img src="https://img.shields.io/badge/dependencies-none-brightgreen.svg" alt="No Dependencies">
  <img src="https://img.shields.io/badge/plugin-Claude%20Code-black.svg" alt="Claude Code Plugin">
</p>

---

## What it does

Type `/research` and Claude stops guessing.

| Constraint | What happens | Why it matters |
|-----------|-------------|----------------|
| **Verify with citations** | Every claim needs a source. No source = `[retracted]` | Removes confident-sounding fiction |
| **Direct quotes for factual grounding** | Extracts exact quotes before analyzing | Kills paraphrase-drift |
| **Chain-of-thought verification** | Step-by-step logic before conclusions | Surfaces bad assumptions |
| **External knowledge restriction** | Only uses info from provided documents | No training data mixed in |

Type `exit research mode` to go back to normal.

## Install

**Plugin (recommended):**

```
claude plugin add ibliminse/research-mode
```

**Manual:**

```bash
git clone https://github.com/ibliminse/research-mode.git ~/.claude/skills/research-mode
```

## Usage

```
/research
```

With a topic:

```
/research what caused the Change Healthcare breach
```

Claude enters research mode immediately — all 4 constraints active. Every claim gets a source or gets cut. Reasoning is shown step by step. No training data mixed with your documents.

## Why not all 7 all the time?

Anthropic published [7 anti-hallucination techniques](https://docs.anthropic.com/en/docs/test-and-evaluate/strengthen-guardrails/reduce-hallucinations). I split them:

**3 should be always on** (no downside):

1. Say "I don't know" — never fill gaps with fiction
2. Don't contradict yourself — catch your own conflicts
3. Self-review before responding — one mental pass before answering

**4 are a toggle** (this plugin):

4-7 add rigor that's valuable for research but would slow down creative work. Citation requirements are overkill for "should I use flexbox or grid." Step-by-step reasoning is noise when you're brainstorming. Sometimes you *want* Claude to use general knowledge.

Research mode is a scalpel, not a lifestyle.

### Want the 3 always-on?

Add this to `~/.claude/rules/` or your `CLAUDE.md`:

```markdown
## Anti-Hallucination (Always On)

1. If you don't have a credible basis for a claim, say so.
   Never fill knowledge gaps with plausible fiction.
   "I don't have enough information" is always a valid answer.

2. When making multiple related claims, verify they're consistent.
   If you catch a contradiction, flag and resolve it before presenting.

3. For non-trivial responses, do one mental pass:
   "Can I actually back up each claim I'm about to make?"
   Cut anything you can't.
```

## How it works

This is a single markdown file (`commands/research.md`) that Claude reads and follows. No dependencies, no build step, no API calls, no external services.

When active, Claude structures output as:

```
Finding: [claim]
Source: [file path, URL, quote number, or named source]
Confidence: high / medium / low
```

Unsupported claims are retracted:

```
[retracted — no source found]
```

## Source

Based on Anthropic's official documentation:
[docs.anthropic.com/en/docs/test-and-evaluate/strengthen-guardrails/reduce-hallucinations](https://docs.anthropic.com/en/docs/test-and-evaluate/strengthen-guardrails/reduce-hallucinations)

## License

MIT -- do whatever you want with it.
