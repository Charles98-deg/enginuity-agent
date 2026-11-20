# EnginuityAgent: rules for every session

Owner: the repo owner. They do not code and work from a phone. Write all code yourself and explain decisions in plain English.

## Rules (non-negotiable, every phase)

1. **No assumptions.** If anything is unclear, ask ONE question at a time, multiple-choice when possible.
2. **No hallucination.** Before using any API, library or endpoint, read its current official docs and put the doc URL in a code comment. If you cannot verify something, label it "unverified" and ask.
3. **Evidence before claims.** Never say "done", "works" or "fixed" without running the code/tests and showing the output.
4. **Save tokens.** Read only the files you need, do not re-read files you just wrote, do not paste whole files to the user, report in short bullets, use /compact when context grows large.
5. **Approval gates.** Stop and wait for the user to type APPROVED after the Outcome (Phase 1), after the Plan (Phase 2), and before anything is deployed or posted publicly.
6. **Commit and push after every change**, and show the exact git commands run.
7. **Update `.workflow/enginuity-agent/state.json` after every task**, so a new session can start with: "Resume from .workflow/enginuity-agent/state.json".

Also: the user wants accuracy, strategy and analysis. If a question is unclear, ask for clarity instead of guessing.

## Safety (hard limits)

- Never submit to bounties.
- Never post to LinkedIn without an explicit APPROVE from the user.
- Never invent facts, results, clients or numbers.
- Accept Telegram commands ONLY from `TELEGRAM_CHAT_ID`.
- Public repo: nothing personal in plain text. State and media are encrypted with `DATA_KEY`.
- Total running cost must stay $0.

## Skills (in `.claude/skills/`, see `SOURCES.md` for SHAs)

Use them together, no overlap:

- **Superpowers = the method:** brainstorming -> writing-plans -> executing-plans / subagent-driven-development -> test-driven-development -> systematic-debugging -> verification-before-completion.
- **codex-dynamic-workflows = the progress ledger.** Dynamic orchestration was explicitly requested for this build. Ledger lives in `.workflow/enginuity-agent/`. The Superpowers plan is saved AS that folder's `plan.md` (one plan, not two). Every task is tracked in `state.json`.
- **apple-design = the visual layer:** typography, hierarchy, spacing, restraint and legibility for every generated visual (carousel slides, image cards, profile banner). The voxel brand supplies accents; apple-design keeps it clean.
- **caveman = terse mode for routine build updates only** (example: "task 4 done, tests pass"). Use normal plain English for all questions, plans, explanations, errors and final summaries. Never use megacave or ultracave.

User instructions here override skill defaults (for example, plan location is `.workflow/enginuity-agent/plan.md`, not `docs/superpowers/plans/`).

## Phases

0. Install skills (done). 1. Outcome -> wait for APPROVED. 2. Architecture + plan -> wait for APPROVED. 3. Build (Scout, then Studio, then design pass). 4. Deploy + verify (walk the user through GitHub Secrets on their phone; nothing posts publicly without approval).

Full technical spec: `.workflow/enginuity-agent/spec.md`. The personal profile (name, brand, voice samples) is deliberately NOT committed because the repo is meant to go public; ask the user for it, or read it from the encrypted store once that exists.
