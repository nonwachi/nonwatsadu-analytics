# นนท์วัสดุ sales analytics: rules for AI assistants

Read this file first, every session. It applies to Claude, Claude Code, ChatGPT, Codex and any other AI tool working in this repo.

## What this repo is

Plans, data requests, queries, analysis code and findings for analyzing the shop's sales to understand business performance. The website, brand and digital-presence work is in a separate repo, `nonwachi/nonwatsadu-website`; do not duplicate its content here.

Who is who: Chi (Okai, Non's dad) is the shop owner and makes every decision about the shop's data and systems. Non (repo owner, based in the US) runs the analysis. The shop's data lives in Express, a Thai accounting program, on the shop's own machines.

## Data rules (from the owner's handover guide, 10 Nov 2026)

1. **No raw business data in this repo or in PR text or issues.** Raw means Express sales, customers, costs, internal prices, email contents, bank documents, credentials, personal data. GitHub and Claude are cloud services.
2. **The owner's current policy: raw data goes into a cloud AI only if Chi approves.** His rule is that raw Express data never goes to a cloud AI unless the data class, destination, method, purpose and his security gate all allow it. Non is asking him for that access (see `docs/PROJECT-PLAN.md`). Until Chi's approval is recorded there, with who, date and exact scope, treat all Express-derived data as not cleared for any cloud AI chat, including Claude.
3. **Data files stay out of the repo, even once approved** (`.gitignore` blocks common data formats). The repo holds schemas, the code that analyzes the data, and findings. Findings may include figures only from data covered by a recorded approval.
4. **No access to the shop's systems.** No NAS, VPN, connector or login is granted. Data arrives only as files the owner chooses to send.
5. **The owner decides.** AI output is advice, a draft or a candidate, never a decision.
6. **Label claims by evidence**, with source and date: VERIFIED-DATED (checked against a file or run), REPORTED (stated, not checked), DESIGN/PILOT, PROPOSED, HOLD, UNKNOWN. Never upgrade a label because someone repeated a claim. Hashes, counts and dates come from tool output, never typed from memory.
7. Check what each field means (for example whether a price includes VAT) before trusting a total or margin.

## Workflow

1. Never push to `main` after the initial scaffold. One small change per branch, open a pull request.
2. Only Non approves and merges. Claude or ChatGPT may merge on Non's behalf only after Non explicitly says to merge that change in chat.
3. Never commit passwords, API keys or tokens. Never copy anything from the family LINE chat export.
4. Ask before publishing or sending anything Non did not request.
5. Decisions, confirmed facts and open-item changes go in `docs/PROJECT-PLAN.md` as short dated lines in the change log. Longer findings go in a new file under `docs/`, linked from the plan.
6. Before a pull request: confirm no secrets and no raw data in the diff, and say in the description what changed.

## Language and tone

- Reply to Non in English, short and not wordy.
- Anything meant for Chi (Non's dad) is Thai only: plain spoken everyday Thai, concise, impersonal (no "I" or "you", no conversational tone), objectives and plan details only. Not formal business wording.
- In Thai text Non calls his dad ป๊า and his aunt โกเล็ก.

## Layout

- `docs/PROJECT-PLAN.md`: source of truth for goal, decisions, open items, change log
- `docs/data-landscape.md`: what data exists, how granular, how to reach it
- `docs/data-request-th.md`: the data request to the owner (Thai)
- `queries/`, `analysis/`: code, added when real work starts
