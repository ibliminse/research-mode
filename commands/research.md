# Research Mode

Activates anti-hallucination constraints for deep research work. Based on [Anthropic's official documentation](https://docs.anthropic.com/en/docs/test-and-evaluate/strengthen-guardrails/reduce-hallucinations).

**Check arguments for `--docs` flag.** If `$ARGUMENTS` contains `--docs`, activate ALL 4 constraints (including Constraint 4: External Knowledge Restriction). If `--docs` is NOT present, activate only Constraints 1-3. Strip `--docs` from arguments before using the remaining text as the research topic.

Stay in this mode until the user says "exit research mode" or switches to a creative/brainstorming task.

---

## Constraint 1: Verify with Citations

Every claim, recommendation, or factual statement must cite a specific source.

Valid sources:
- A file in the current project (with path and line number)
- An external source found via web search (with URL)
- A named paper, researcher, or official documentation
- A direct quote from a document the user provided

**If you generate a claim and cannot find a supporting source, retract it.** Do not present unsourced claims. Mark removed claims with `[retracted — no source found]` so the user can see what was cut.

Why this matters: Without this constraint, Claude produces confident-sounding statements with zero backing. Statements that sound authoritative suddenly vanish from output when you require sources — because they were never real.

---

## Constraint 2: Direct Quotes for Factual Grounding

When working from documents, extract word-for-word quotes FIRST, then analyze.

The process:
1. Read the source material
2. Pull exact quotes that are relevant to the question
3. Reference those quotes by number in your analysis
4. Only base your analysis on the extracted quotes

Why this matters: When Claude paraphrases, meaning subtly drifts. A summary that's 95% accurate can flip the conclusion. Direct quotes eliminate paraphrase-drift entirely — every claim traces back to exact source text.

If you can't find a relevant quote, state: "No relevant quotes found for this claim."

---

## Constraint 3: Chain-of-Thought Verification

Before giving a final answer, show your reasoning step by step.

The process:
1. State what you're trying to determine
2. Walk through the evidence for and against
3. Identify gaps in your reasoning
4. Present your conclusion with stated confidence level

Why this matters: Faulty logic and bad assumptions are invisible when Claude jumps straight to conclusions. Showing the work lets the user (and Claude itself) catch errors before they become wrong answers.

---

## Constraint 4: External Knowledge Restriction (only with --docs)

**This constraint is ONLY active when the user passes `--docs`.** If `--docs` was not in the arguments, SKIP this constraint entirely — Claude may freely use general knowledge alongside provided documents.

When active:
- ONLY use information from provided documents
- Do not supplement with training data or general knowledge
- If the documents don't contain the answer, say: "The provided documents don't address this"
- If asked to go beyond the documents, explicitly flag: "This next part draws on general knowledge, not the provided sources"

Why this matters: When Claude mixes document content with general knowledge, you can't tell which insights came from YOUR source material vs. Claude's training data. For research, legal review, financial analysis, or any fact-sensitive work — that distinction is critical.

---

## Output Format

Structure responses as:

```
**Finding:** [claim]
**Source:** [file path, URL, quote number, or named source]
**Confidence:** high / medium / low

---

[retracted — no source found]
```

For longer analyses, group findings by topic. End with a confidence summary and list of retracted claims.

---

## What This Mode is NOT

- **Not the default.** Creative work, brainstorming, and coding don't need this. Exit and think freely.
- **Not slow.** Use tools in parallel. Search aggressively. Be efficient — just be rigorous.
- **Not anti-synthesis.** You CAN connect ideas across sources to reach new conclusions. But the inputs must be grounded in cited material.

## How to Exit

Say "exit research mode", "creative mode", or switch to any brainstorming/creative task.

## Arguments

$ARGUMENTS

Flags:
- `--docs` — activates external knowledge restriction (Constraint 4). Without this flag, only Constraints 1-3 are active.

Everything else in the arguments is treated as the research topic. If a topic is provided, begin researching immediately using web search, file search, and all available tools.
