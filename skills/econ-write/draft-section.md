Methodology encoded from: McCloskey (Economical Writing), Cochrane (Writing Tips), Angrist & Pischke (MHE/Mastering Metrics chapter structure), Huntington-Klein (The Effect pedagogy), Cunningham (Mixtape voice), Sant'Anna (adversarial critic-fixer pattern), Thomson (Guide for the Young Economist), AER Style Guide.

### /draft-section — Turn notes into polished academic prose

**Trigger**: "draft section on [topic]", "write up my notes on [topic]", "turn these notes into prose"

**Inputs accepted**:
- A section from `./projects/[project]/outline.md`
- Paper snapshot files from `./literature/`
- Raw notes from `./notes/` or `./fleeting/`
- An explicit topic + bullet points from the user

**Workflow**:

1. **Gather context**: Read the outline section, identify which paper snapshots and notes are relevant. If a `STYLE_GUIDE.md` exists in the project directory, load it for voice/convention rules.

2. **Determine output type** (infer or ask):
   - **Journal paper section**: Formal, third-person, dense, contribution-focused
   - **Textbook chapter section**: Pedagogical, second-person okay, question-first, intuition-before-math
   - **Consulting report section**: Executive-oriented, bottom-line-up-front, policy implications prominent

3. **Draft following the economics writing rules**:

   **Structure rules (Cochrane)**:
   - Lead with the most important claim — triangular/newspaper style
   - No throat-clearing: delete "It is well known that...", "The literature has long debated...", "In this section, we will..."
   - One idea per paragraph; topic sentence carries the claim
   - Every paragraph must advance the argument, not merely describe

   **Prose rules (McCloskey)**:
   - Active voice default; passive only when the agent is genuinely unknown
   - No "naked this" — every "this" must be followed by a noun ("this result", "this assumption", not just "this")
   - Concrete nouns over abstractions: "wages fell 3%" not "there was a negative impact on labor market outcomes"
   - Vary sentence length; short sentences for emphasis, longer for elaboration
   - Cut 25% of words from every first draft — if you can remove a word without losing meaning, remove it

   **Math integration rules (Knuth + AER Style Guide)**:
   - State the intuition in plain language BEFORE the equation
   - Present the equation on its own line, numbered
   - Walk through each term: "where Y_i denotes [outcome] for individual i, D_i is the treatment indicator, and X_i is a vector of covariates"
   - Explain what the equation DOES: "This expression says that the treatment effect is identified by comparing outcomes of treated and untreated individuals who share the same value of X"
   - Connect back to the empirical example
   - Never start a sentence with a symbol
   - Separate symbols with words: write "Consider S_q, where q < p" not "Consider S_q, q < p"
   - Equations are English sentences — punctuate them (comma if clause continues, period if it ends)
   - Italics for scalars, bold for vectors/matrices, blackboard bold only for ℝ, ℤ, ℕ

   **Citation rules**:
   - Author-date in-text: "Angrist and Krueger (1991) find..." or "(Angrist and Krueger 1991)"
   - 1-2 authors named; 3+ use "et al." in text
   - Use `[@citekey]` for pandoc/Quarto citeproc compatibility
   - Every empirical claim must have a citation; every citation must serve a purpose
   - For textbooks: cite to credit and direct readers, not to establish novelty

   **For textbook/practitioner guide sections specifically**:
   - Open with a causal question or empirical puzzle, not a definition
   - Use the Angrist-Pischke chapter template: Hook → Intuition → Key example → Formal framework → Code/implementation → Extensions → Exercises
   - Separate identification from estimation when possible (Huntington-Klein principle)
   - Include "equation walks" for every displayed equation
   - Integrate code demonstrations alongside the prose (mark language with fenced code blocks: ```r, ```python, ```stata)
   - Use panel-tabset syntax for multi-language code: `::: {.panel-tabset}` for Quarto

4. **Output**: Save the drafted section to `./projects/[project]/drafts/[section-name].qmd` (or `.md` if not using Quarto)

5. **Post-draft check**: Run the quick self-check (see /self-review) on the drafted section before confirming.
