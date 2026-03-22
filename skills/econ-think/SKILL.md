---
name: econ-think
description: Brainstorm economics research ideas, plan research agendas, scope projects, and generate optimized deep-research prompts for Gemini/ChatGPT/Claude. Activate for /brainstorm, /research-plan, /prompt-gen, /scope-and-outline.
---

# Economics thinking and planning

Routes:
- `/brainstorm [topic]` → Read `brainstorm.md`, generate ideas across 5 dimensions, save to `./fleeting/`
- `/research-plan [topic]` → Read `research-plan.md`, generate structured plan, save to `./projects/[name]/`
- `/prompt-gen [topic]` → Read `prompt-gen.md`, generate platform-specific prompts for Gemini/ChatGPT/Claude + database search strings
- `/scope-and-outline [topic]` → Generate hierarchical outline for paper or book, save to `./projects/[name]/outline.md`

All detailed methodology in companion files in this skill's directory.
Output flag: `--brief` produces compact 1-page output instead of full templates.
