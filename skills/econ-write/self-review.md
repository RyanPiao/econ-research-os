### /self-review — Adversarial quality review of draft text

**Trigger**: "review my draft", "check this chapter", "self-review [file]", "referee this"

This mode implements Sant'Anna's adversarial critic pattern and Cunningham's Referee 2 approach. The skill acts as a hostile but constructive reviewer.

**Workflow**:

1. **Read the draft** (file path or current chapter)

2. **Run 7 review lenses sequentially**:

   **Lens 1: Argument structure (Cochrane test)**
   - Can you state the central claim of this section in one sentence?
   - Does the first paragraph contain the main point, or is it buried?
   - Summarize each paragraph in one sentence — does the string form a coherent argument? (Thomson test)
   - Is there throat-clearing that can be deleted?
   - Score: /10

   **Lens 2: Pedagogical clarity (for textbook chapters)**
   - Does each section open with a question or puzzle, not a definition?
   - Is the intuition presented BEFORE the math?
   - Would a smart undergraduate understand the key idea without the equations?
   - Are there concrete examples (names, numbers, places) or just abstractions?
   - Does the chapter build from simple to complex?
   - Score: /10

   **Lens 3: Mathematical accuracy and presentation**
   - Is every symbol defined on first use?
   - Check `NOTATION.md` — is notation consistent with previous chapters?
   - Does every equation have a walk-through?
   - Are subscripts/superscripts meaningful and minimal?
   - Are equations punctuated as English sentences?
   - Do any special cases produce intuitive results? (Sanity check)
   - Score: /10

   **Lens 4: Prose quality (McCloskey rules)**
   - Flag passive voice (acceptable rate: <20% of sentences)
   - Flag "naked this" (every instance)
   - Flag vague quantifiers ("significant impact" without a number)
   - Flag unnecessary hedging ("it could be argued that perhaps...")
   - Flag jargon used without definition
   - Flag sentences over 40 words (candidates for splitting)
   - Estimate: what % of words could be cut?
   - Score: /10

   **Lens 5: Citation and evidence integrity**
   - Does every empirical claim have a citation?
   - Are all `[@citekey]` entries present in `references.bib`?
   - Flag any claim that sounds specific but lacks a source (hallucination risk)
   - Are citations primary sources or secondary summaries?
   - For textbook: are canonical papers cited for credit + direction?
   - Score: /10

   **Lens 6: Cross-reference and notation consistency**
   - Do all `@fig-`, `@eq-`, `@sec-`, `@tbl-` references resolve?
   - Scan for notation used differently than in `NOTATION.md`
   - Check: are the same variables/concepts called the same thing across sections?
   - Flag: any term introduced here that contradicts an earlier chapter
   - Score: /10

   **Lens 7: Voice and tone consistency**
   - Compare the register (formality, humor, pronoun use) with Chapter 1
   - Flag shifts in voice: sections that suddenly sound different
   - Check: is the reader addressed consistently (you/we/the reader)?
   - Flag: AI-sounding phrases ("It is important to note that...", "In conclusion...", "delve into", "it's worth noting")
   - Score: /10

3. **Generate review report**:

   ```markdown
   ## Self-Review Report: [File]
   **Date**: [YYYY-MM-DD]
   **Overall score**: [sum]/70

   ### Scores by lens
   | Lens | Score | Key issues |
   |------|-------|------------|
   | Argument structure | /10 | |
   | Pedagogical clarity | /10 | |
   | Math accuracy | /10 | |
   | Prose quality | /10 | |
   | Citation integrity | /10 | |
   | Cross-references | /10 | |
   | Voice consistency | /10 | |

   ### Critical issues (must fix)
   1. [Issue + location + suggested fix]

   ### Important issues (should fix)
   1. [Issue + location + suggested fix]

   ### Minor issues (nice to fix)
   1. [Issue + location + suggested fix]

   ### Strengths
   1. [What works well]

   ### Quality gate
   - [ ] Score ≥ 50/70: Commit-safe (okay to save and continue)
   - [ ] Score ≥ 60/70: Review-ready (okay to share with readers)
   - [ ] Score ≥ 65/70: Near-final (minor polish only)

   ### Recommended next action
   [Specific instruction: "Rewrite section 3.4 opening to lead
   with the result instead of background" or "Add equation walk
   for Eq. 7" or "Replace passive voice in paragraphs 2, 5, 9"]
   ```

4. **Save**: `./projects/[project]/reviews/review-[filename]-[date].md`

5. **Auto-fix mode** (if user requests): After generating the review, apply fixes for mechanical issues (naked this, passive voice, missing punctuation on equations) while leaving substantive issues for the user.
