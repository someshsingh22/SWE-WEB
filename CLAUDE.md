# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a LaTeX academic paper repository for **SWE-WEB**, a NeurIPS 2026 submission. The paper introduces a benchmark for evaluating autonomous coding agents on improving real websites' Core Web Vitals (LCP, CLS, INP).

The repo was imported from Overleaf.

## Build

Compile with `pdflatex` + `bibtex` (or `latexmk`):

```bash
pdflatex neurips_2026.tex
bibtex neurips_2026
pdflatex neurips_2026.tex
pdflatex neurips_2026.tex
```

Or with latexmk:

```bash
latexmk -pdf neurips_2026.tex
```

Bibliography uses `plainnat` style with `sample-base.bib`.

## Document Structure

- **`neurips_2026.tex`** — Main entry point (NeurIPS format, active submission)
- **`sample-sigconf.tex`** — Earlier ACM SIGCONF version (not actively used)
- **`pages/`** — Individual sections included via `\input{}`:
  - `intro.tex`, `method.tex`, `experiments.tex`, `results_discussions.tex`, `conclusion.tex`, `related_work.tex`, `limitation.tex`, `todo.tex`
  - `appendix/` — Supplementary sections (visual regression, CWV measurement, prompts, etc.)
- **`figures/`** — All figures (PDF and PNG)

## Custom LaTeX Macros

Defined in `neurips_2026.tex`:

- **Author annotations**: `\somesh{}`, `\arnav{}`, `\ayush{}`, `\manaswi{}` — color-coded inline comments
- **`\todo{}`** — magenta TODO markers
- **`\OurBench`** — expands to "SWE-WEB " (benchmark name shorthand)
- **`\cmark`**, **`\xmark`**, **`\pmark`** — check, cross, and partial markers for tables

## Key Conventions

- The largest content files are `pages/method.tex` (~33KB) and `pages/todo.tex` (~65KB working notes)
- Figures are a mix of PDF (diagrams) and PNG (plots/charts)
- The paper has two target formats; only `neurips_2026.tex` is the active submission

# SWE-WEB Paper -- Writing Guidelines

## Primary Goal

This is the LaTeX repository for the SWE-WEB / SWE-Experience-Bench paper. The paper introduces an execution-based benchmark for evaluating autonomous coding agents on real-world web performance optimization.

The benchmark tests whether agents can improve **Core Web Vitals (CWV)**: LCP, CLS, and INP, across mobile and desktop settings. Agents edit real repositories; the harness hosts the patched site; browser-based measurements determine whether the patch improves user-facing performance.

**The central question**: Can autonomous coding agents reliably improve user-facing web performance without degrading other dimensions of experience?

## The Maxims of Writing

These maxims govern ALL prose written or edited in this repository. They should be followed as much as possible.

### 1. Reading -- "Read before you write."

Before writing or rewriting any section, read the surrounding paragraphs, relevant tables, and cited work. Every claim about prior benchmarks, web performance metrics, or coding agents must reflect what the cited work actually demonstrates.

Read with the current paragraph's purpose in mind. The same related work matters differently depending on whether the section is motivating execution-based evaluation, defining CWV, or comparing SWE-WEB to software-engineering benchmarks.

Even after you edit, review the content up to this point and check that every maxim here is followed as much as possible.

### 2. Quantity -- "Do not write more than what is required."

The paper has a strict page limit. Every sentence must earn its place.

- If a sentence can be cut without losing the argument's logical chain, cut it.
- Do not explain background the ML audience already knows.
- Explain CWV only to the extent needed for the paper's argument.
- Hosting details, framework-specific scripts, retry logic, and measurement implementation details usually belong in the appendix.
- Avoid throat-clearing openings such as "In recent years..." or "It is well known that...". Start with the claim.

### 3. Quality -- "Do not deceive the reviewer."

Every empirical claim must be backed by an experiment, measurement, or ablation that is actually in the paper.

Specifically:
- Never claim a result is "significant" without variance or statistical testing.
- Never write "substantially outperforms" when the margin is within noise.
- Never imply that CWV captures all aspects of user experience.
- Never argue from LCP alone. The benchmark evaluates LCP, CLS, and INP jointly across mobile and desktop.
- Never claim that a patch improves a site unless post-patch measurement verifies it.
- Do not present reward hacking as impossible. State what the Pareto constraint makes harder to game, and what uncertainty remains.
- Keep the claim envelope consistent throughout: do not overclaim in the intro and retreat in the experiments.

### 4. Language -- "Do not play with words. Avoid obscurity."

ML papers demand terminological precision.

Rules:
- Use the same term for the same concept throughout. If it is "Core Web Vitals," do not later call it "page-speed score" unless referring to something different.
- If it is "Pareto improvement," do not later call it "success" without defining the connection.
- Define a term the first time it appears. After definition, use it without re-explanation.
- Avoid hedging stacks such as "might potentially be able to somewhat improve."
- Prefer concrete nouns over abstractions: "the agent adds image dimensions and lazy loading" over "the method performs frontend optimization."
- Written academic English is not spoken English. Avoid colloquialisms, contractions, and filler phrases.

### 5. Relevance -- "Every sentence must serve the argument and must be motivated."

Before writing a paragraph, ask: *what claim does this paragraph establish, and which part of the paper's argument chain does it belong to?*

The argument chain is:

1. Coding-agent benchmarks mostly evaluate functional correctness.
2. Real software quality also depends on user-facing non-functional properties.
3. Web performance is a concrete, measurable instance of this problem.
4. CWV measures complementary dimensions of loading, stability, and responsiveness.
5. Optimizing one metric can degrade another, so evaluation must be multi-metric and execution-based.
6. SWE-WEB evaluates agents on real repositories using browser-measured CWV outcomes.

In the Introduction, the problem is *execution-based evaluation of coding agents for user-facing web performance*. It is not a general survey of web development, Lighthouse, or LLM scaling.

### 6. Metaphor and Analogy -- "Use images to communicate complex ideas."

Technical ML papers benefit from well-chosen analogies, but only when they clarify the benchmark.

Good analogies:
- SWE-WEB treats web performance like an execution-tested software property: a patch must run, be hosted, and be measured.
- The Pareto criterion prevents agents from winning by improving one metric while sacrificing another.
- Lab measurement is a controlled proxy; field data helps validate whether the relative ordering is meaningful.

Use analogies to build intuition, then immediately give the technical definition. One good analogy per major concept is enough.

### 7. Logic -- "The paper is not the sum of its sections."

The paper must read as a single logical argument, not a stapled collection of datasets, scripts, and agent results.

The structure:

- **Introduction**: Establish the question: how should we evaluate coding agents on user-facing, execution-measured software quality? Motivate web performance and CWV. End with contributions.
- **Benchmark / Method**: Define repository selection, hosting, measurement, aggregation, and Pareto success.
- **Experiments**: Evaluate agents, metric trade-offs, costs, patch types, and failure modes.
- **Conclusion**: Honestly state what SWE-WEB shows, what it does not show, and what remains open.

Logical discipline means:
- If the intro claims single-metric optimization is insufficient, the method must define joint multi-metric evaluation.
- If the paper claims lab measurements are meaningful, it must include the CrUX/lab-field validation.
- If the paper claims benchmark diversity, it must report repository scale, framework coverage, and page coverage.
- Avoid hasty generalization: success on web performance does not imply general software-engineering competence.

### 8. Line of Thought -- "Avoid digression."

Each section has one job. Each paragraph within that section advances that job by exactly one step.

Do not:
- Discuss related work inside the method section.
- Re-motivate the problem inside every experiment subsection.
- Introduce new benchmark design choices in the results section.
- Put measurement caveats in the middle of interpreting agent performance.
- Start a paragraph about repository selection and end it discussing agent cost.

The reader should be able to extract a one-sentence summary from every paragraph. If a paragraph resists summarization, it is doing too many things.

### 9. Meaning -- "What does this paragraph actually say?"

Every paragraph must pass four tests:
- **Coherence**: Does each sentence follow from the previous one? Are the logical connectives accurate?
- **Syntax and Semantics**: Is the grammar correct? Do the words mean what you think they mean? "Significant" has a specific meaning. "Robust" needs evidence.
- **Purpose**: Could a reviewer annotate this paragraph with its role in the argument? Motivation / Gap / Method / Result / Interpretation / Limitation.
- **Gestalt**: Does the paragraph contribute to a cohesive narrative: execution-based evaluation reveals whether coding agents can improve user-facing web performance without metric trade-offs?

---

## Positioning Rules

These rules are mandatory:

1. **Lead with the ML/software-engineering question**, not the dataset size or the web domain. The question is: "Can coding agents optimize user-facing, non-functional software quality under execution-based evaluation?"
2. **Web performance is the controlled testbed**, not the whole contribution. CWV is used because it gives standardized, browser-level, user-facing signals that are objective, reproducible, and independent of unit-test design.
3. **Name the transferable recipe**: real repositories + executable tasks + browser measurement + multi-metric Pareto scoring.
4. **Multi-metric breadth is the shield** against "this is just LCP." SWE-WEB evaluates LCP, CLS, and INP across mobile and desktop.
5. **NEVER argue that LCP alone captures user experience.** LCP is a practical proxy for perceived loading, not a complete measure of quality.
6. **Highlight execution-based validation**: agents patch repositories, sites are hosted, and CWV is re-measured. The measurement modality is a real browser, not a unit test, a performance microbenchmark, or a static analyzer.
7. **Do not overstate reward-hacking conclusions.** Low observed prevalence is not proof that gaming is impossible.
8. **Frame SWE-WEB as complementary to SWE-bench-style evaluations.** The SWE-bench ecosystem (SWE-bench, SWE-bench M, SWE-Gym, SWE-smith, SWE-rebench) tests functional correctness on Python repositories. SWE-WEB tests user-facing performance optimization on real web repositories. These are orthogonal evaluation axes. Do NOT position SWE-WEB as a variant or extension of SWE-bench.
9. **Distinguish from GSO.** GSO (NeurIPS 2025) is the closest related work: it also evaluates coding agents on a non-functional property (runtime speed) using execution-based measurement. The distinctions are load-bearing: (a) SWE-WEB measures user-facing browser metrics (LCP, CLS, INP), not computational speed; (b) measurement requires hosting the patched site and running a real browser, not executing a performance test; (c) SWE-WEB enforces multi-metric Pareto success across mobile and desktop — agents cannot win by trading one metric against another; (d) GSO's tasks are derived from commit histories in systems codebases; SWE-WEB's tasks are real production web repositories evaluated end-to-end. Acknowledge GSO as concurrent/prior work; do not ignore it.
10. **Distinguish from BaxBench.** BaxBench (ICML 2025) evaluates non-functional properties (correctness + security) of LLM-generated backends. SWE-WEB differs on two axes: (a) agents edit existing production repositories, not generate new applications from scratch; (b) the quality signal is browser-measured user experience, not security exploit success or API test pass rate.
11. **Contamination resistance is a structural advantage.** Unlike SWE-bench (where solution leakage and weak tests inflate scores), SWE-WEB's success signal is a browser-measured CWV improvement on a hosted site. It cannot be gamed by memorizing patches or exploiting incomplete test coverage. State this explicitly when discussing benchmark validity.
12. **The Pareto criterion is the methodological novelty.** No existing benchmark for coding agents uses multi-metric Pareto scoring. Name it as the mechanism that prevents single-metric gaming and enforces joint improvement across LCP, CLS, and INP on both mobile and desktop.

---

## Writing Method

When writing or rewriting a section, follow this process:

1. **State the paragraph's job** in one sentence before writing it.
2. **Identify the evidence** that supports the claim: experiment, citation, benchmark design, metric definition, or table.
3. **Write the paragraph** as: Topic sentence → Evidence/reasoning → Implication for the next paragraph.
4. **Cut** any sentence that does not serve the claim.
5. **Check connectives**: does "however" actually introduce a contrast? Does "therefore" actually follow from the premises?
6. **Read it aloud**: if it sounds like it is trying to impress rather than inform, rewrite it to inform.
7. Avoid clichés, excessive em dashes, repeated "not just X but Y" phrasing, and vague words such as "holistic", "seamless", or "robust" unless precisely supported.

---

## LaTeX Conventions

- Comment macros: `\somesh{}`, `\yaman{}`, `\arnav{}` for author notes.
- Placeholder citations: use `\cite{XXX}` with a descriptive key for missing references.
- Use `Core Web Vitals (CWV)` on first mention, then `CWV`.
- Use `LCP`, `CLS`, and `INP` consistently.
- Use `Pareto improvement` or `Pareto success` consistently for the joint metric-device criterion.
- Keep repository counts, framework counts, webpage counts, and agent names consistent across sections.