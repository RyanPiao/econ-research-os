### /book-chapter — Draft or revise a full book chapter

**Trigger**: "write chapter [N] on [topic]", "draft the [topic] chapter", "expand my outline for chapter [N]"

**Inputs**:
- Chapter entry from `./projects/[project]/outline.md`
- All paper snapshots tagged with this chapter's topic
- Any existing draft sections in `./projects/[project]/drafts/`
- `STYLE_GUIDE.md` and `NOTATION.md` if they exist

**Workflow**:

1. **Load chapter context**:
   - Read the outline for this chapter (argument, key papers, dependencies)
   - Read `NOTATION.md` for symbol consistency (create if doesn't exist)
   - Read `STYLE_GUIDE.md` for voice conventions (create if doesn't exist)
   - Check which previous chapters exist (for cross-reference consistency)

2. **Plan the chapter structure** following the practitioner guide template:

   ```markdown
   # Chapter [N]: [Title]

   ## [N].1 Opening hook
   [A real-world causal question, empirical puzzle, or policy dilemma
   that motivates the entire chapter. 1-2 paragraphs. Name specific
   people, places, numbers. Make the reader care.]

   ## [N].2 The intuition
   [Plain-language explanation of the core idea. Why does this method
   exist? What problem does it solve that simpler approaches can't?
   Use analogies, thought experiments, or causal diagrams. NO equations
   in this section.]

   ## [N].3 The canonical example
   [One empirical paper that perfectly illustrates the method.
   Tell the story: what was the question, what was the clever design,
   what did they find? Reference the specific tables/figures.]

   ## [N].4 The formal framework
   [Mathematical presentation. Start with the simplest version
   (two-period, two-group) before the general case. Every equation
   gets a full walk-through. Build from the intuition section —
   the reader should recognize the math as formalizing what they
   already understand.]

   ## [N].5 Implementation
   [How to actually do this. Code in R/Python/Stata showing the
   method applied to the canonical example's data (or simulated data).
   Walk through the code, explain the output, connect back to the
   formal framework.]

   ## [N].6 Extensions and practical issues
   [Variations, diagnostics, common mistakes, recent methodological
   advances. When does the method break? What assumptions are people
   most often wrong about?]

   ## [N].7 Further applications
   [2-3 additional empirical examples from different fields.
   Brief treatment — show the method's breadth.]

   ## [N].8 Summary and key takeaways
   [Numbered list of 5-7 key points. What must the reader remember?]

   ## [N].9 Exercises
   [3-5 conceptual questions + 2-3 applied problems with data]
   ```

3. **Draft each section sequentially**, following the /draft-section rules for prose quality.

4. **Cross-reference management**:
   - Use Quarto cross-ref syntax: `@fig-[label]`, `@eq-[label]`, `@sec-[label]`, `@tbl-[label]`
   - Reference previous chapters: "As we saw in @sec-regression, the OLS estimator..."
   - Check that all referenced figures, tables, equations actually exist
   - Maintain a running notation table and verify consistency with `NOTATION.md`

5. **Voice consistency checks** during drafting:
   - Re-read the opening hook of Chapter 1 before writing any new chapter — calibrate the voice
   - Check: Am I using the same level of formality? Same pronoun convention? Same humor register?
   - Check: Are my equation walks following the same pattern (intuition → equation → term-by-term → connection)?
   - If `STYLE_GUIDE.md` exists, verify compliance

6. **Save outputs**:
   ```bash
   # Chapter file
   ./projects/[project]/chapters/[NN]-[short-title].qmd

   # Update notation table
   ./projects/[project]/NOTATION.md

   # Update style guide if new conventions were established
   ./projects/[project]/STYLE_GUIDE.md
   ```

7. **Generate chapter metadata**:
   ```markdown
   ---
   chapter: [N]
   title: "[Chapter Title]"
   status: [first-draft | revised | review-ready | final]
   word_count: [N]
   equations: [N]
   figures: [N]
   tables: [N]
   code_blocks: [N]
   citations: [list of citekeys used]
   cross_refs_to: [list of other chapters referenced]
   cross_refs_from: [list of chapters that reference this one]
   last_edited: [YYYY-MM-DD]
   ---
   ```
