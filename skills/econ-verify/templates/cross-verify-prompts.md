## Cross-Verification Prompts: [Chapter/Paper Title]
**Generated**: [YYYY-MM-DD]
**Purpose**: Independent verification of substance and logic

---

### For Google Gemini Deep Research
[Gemini strength: broad literature verification, finding contradicting evidence]

**Prompt 1 — Claim verification against literature**:
```
I am writing an economics [textbook chapter / research paper] that
makes the following key claims. Please verify each claim against
the published economics literature and flag any that are:
(a) contradicted by well-known results
(b) stated more strongly than the evidence supports
(c) missing important caveats or qualifications

Claims to verify:
1. [Claim 1 — copy exact sentence from manuscript]
2. [Claim 2]
3. [Claim 3]
[... up to 10 key claims]

For each claim, provide:
- Whether the published literature supports, contradicts, or is
  mixed on this claim
- The 2-3 most relevant papers (with full citations)
- Any important caveats the author should add
```

**Prompt 2 — Methodology check**:
```
I am using [SPECIFIC METHOD: e.g., difference-in-differences with
staggered treatment timing] to estimate [SPECIFIC EFFECT].

My identification assumptions are:
1. [Assumption 1: e.g., parallel trends in absence of treatment]
2. [Assumption 2: e.g., no anticipation effects]
3. [Assumption 3: e.g., no spillovers across units]

My data covers [SETTING: population, time period, geography].

Please evaluate:
1. Are these the correct assumptions for this method? Am I missing any?
2. Are there well-known papers that show these assumptions fail in
   settings similar to mine?
3. What diagnostic tests should I run that I might have overlooked?
4. Are there recent methodological advances (2022-2026) that improve
   on this approach for my specific setting?
5. What would a skeptical referee's strongest objection be?
```

---

### For ChatGPT Deep Research
[ChatGPT strength: finding specific facts, checking numbers, policy details]

**Prompt 1 — Factual and numerical verification**:
```
Please fact-check the following specific claims from my economics
manuscript. For each, verify whether the stated number/fact is
accurate by searching for the original source:

1. "[Specific factual claim with number — e.g., 'The federal
   minimum wage was last raised to $7.25 in July 2009']"
2. "[Another claim — e.g., 'Card and Krueger (1994) surveyed
   410 fast-food restaurants in New Jersey and Pennsylvania']"
3. "[Data claim — e.g., 'US GDP growth averaged 2.3% annually
   from 2010-2019']"
[... up to 15 factual claims]

For each:
- Is the stated fact/number correct?
- If not, what is the correct number and what is the source?
- If approximately correct, note the exact figure and source
```

**Prompt 2 — Citation verification**:
```
I need to verify that the following citations in my economics
manuscript are real papers that actually exist. For each, please
confirm:
(a) The paper exists with these authors, title, year, and journal
(b) The paper's actual finding matches my characterization

Citations to verify:
1. [Author(s) (Year). "Title." Journal.] — I cite this as showing
   "[your characterization of the finding]"
2. [Next citation + your characterization]
[... up to 20 citations]

Flag any citations that:
- Do not appear to exist (possible hallucination from AI-assisted writing)
- Exist but the finding doesn't match my characterization
- Have been published in a different journal or year than I state
- Have a more recent published version I should cite instead
```

---

### For Claude Deep Research
[Claude strength: logical consistency, argument structure, identifying subtle flaws]

**Prompt 1 — Logic and argument audit**:
```
I am writing a [textbook chapter / paper] arguing that [MAIN THESIS].

Here is my argument structure:
1. [Step 1 of the argument]
2. [Step 2]
3. [Step 3]
...
N. Therefore, [CONCLUSION]

Please perform a rigorous logical audit:
1. Does each step follow from the previous one?
2. Are there hidden assumptions I haven't stated?
3. Where is the argument weakest — which step would a skeptic
   attack first?
4. Are there alternative explanations for my evidence that I
   haven't addressed?
5. Does my conclusion follow from what I've actually shown, or
   am I overclaiming?
6. If you were Referee 2 at a top economics journal, what would
   be your single most devastating criticism?

Be adversarial. Don't give me the benefit of the doubt.
```

**Prompt 2 — Equation and derivation check**:
```
Please verify the following mathematical derivation from my
economics manuscript. Check each step for correctness:

Starting point: [Equation 1]
Step 1: [Manipulation or assumption applied]
Result: [Equation 2]
Step 2: [Next manipulation]
Result: [Equation 3]
...
Final result: [Key equation]

For each step:
1. Is the algebraic manipulation correct?
2. Is the economic assumption valid?
3. Does the notation remain consistent?
4. What happens in the special case where [key parameter] = 0?
   Does the result reduce to something intuitive?
5. Are there any sign errors or dropped terms?
```

---

### Search queries for manual verification
[Ready-to-paste for spot-checking specific claims]

**Google Scholar** (verify a citation exists):
- `"[exact paper title]" author:[last name]`

**Semantic Scholar** (verify citation count / impact):
- Search title → check year, journal, citation count

**FRED** (verify a macro statistic):
- https://fred.stlouisfed.org/series/[SERIES_ID]

**BLS / Census** (verify a labor/demographic statistic):
- https://data.bls.gov/
- https://data.census.gov/
