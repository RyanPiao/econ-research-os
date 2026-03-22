---
name: econ-verify
description: Adversarial verification of economics manuscripts. Checks mathematical correctness, causal logic, citation validity, code-output consistency, and overclaiming. Generates cross-verification prompts for Gemini/ChatGPT. Activate for /verify, /cross-verify, /verify-code.
---

# Economics verification and cross-model review

This skill checks TRUTH and LOGIC, not style. Use /self-review for prose quality.

Routes:
- `/verify [file]` → Read `verify.md`. Run 6 adversarial lenses: (1) math correctness, (2) empirical claims vs output, (3) causal logic and identification, (4) citation verification, (5) code-pipeline integrity, (6) overclaiming. Produce scored report with PASS/REVISE/MAJOR REVISION verdict.
- `/cross-verify [file]` → Read `cross-verify.md`. Generate targeted prompts: Gemini (literature + methodology check), ChatGPT (fact + citation verification), Claude (logic + derivation audit). Plus manual search queries.
- `/verify-code [script]` → Read-only code review: specification match, SE clustering, random seeds, hardcoded paths, edge cases.

Flags: `--lens math` to run single lens. `--diff` to verify only changed sections since last run.
