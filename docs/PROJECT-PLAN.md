# Sales analytics: project plan and source of truth

Read this first in every new chat, after `AGENTS.md`. Last updated: 2026-11-10. Keep dated notes short.

## 1. Goal

Understand the business performance of นนท์วัสดุ (Nonwatsadu Plus Co., Ltd.) from its sales data, urgently and in detail, over time. Non wants the request to Chi (Non's dad, the owner) sent as soon as possible. Digital presence work continues separately in `nonwachi/nonwatsadu-website`.

## 2. Business context (short; REPORTED by Non, see the website repo plan)

- Family building-materials shop in Mueang Lampang, Thailand. About 80% of customers are contractors and tradespeople; paint draws homeowners.
- Products: paint, lumber and boards, furniture-making supplies, steel, cement. Offers vetted credit and delivery. No cutting service.
- Non runs this from the US; Chi runs the shop and the shop's systems.

## 3. Sources

- NONT PLUS Successor Handover & Onboarding Guide, Joint R1d, 10 Nov 2026, written by Chi's AI team (77 = Claude, Shimi = ChatGPT) and emailed to Non, Jom and Jean. Not stored here: Chi approves each recipient's copy. Findings from it are in `data-landscape.md`.
- Web checks, 10 Nov 2026: public sources did not show how Express stores or exports data. UNKNOWN until Chi answers.

## 3a. Business requirement (Non, 2026-11-10)

Explain how the business performs and why, to decide where to focus: sales over time and seasonality; which products and brands bring revenue and profit; who the customers are, how concentrated, who returns; where margin leaks (discounts, returns, terms); credit exposure. This needs transaction-level history, not summaries. Chi's existing pipeline serves operations and pricing, not performance analysis. Also needed: a repeatable monthly feed rather than a one-off, agreed definitions (VAT, returns, what counts as cost), and reconciliation to Chi's books.

## 4. Decisions

- 2026-11-10 (Non): analytics work gets its own repo, not the website repo.
- 2026-11-10 (Non): the data request goes to Chi as soon as possible.
- 2026-11-10 (Non): ask for all the data including customers, with a shareable-data fallback.
- 2026-11-10 (Non): before access and permissions are sorted, ask first for the full data schema, then highlight the tables and fields that matter and request only those. Also find out whether Thai text stored in Express stays legible in other environments.

## 5. The request (draft: `data-request-th.md`)

**Step 1, now:** the full data schema, no data rows. Tables, fields (name, type, length, meaning), relationships, row counts and date ranges, Express version and storage format, how Thai text is stored, a 20-name product sample to test Thai in other tools, any Express documentation, and whether older copies or backups exist and where.

**Step 2, after Non reviews the schema:** Non marks the tables and fields that matter. The request then covers only those, as a read-only copy as far back as it goes, customer data included if needed.

**Handling terms, proposed for Chi to rule on:** class confidential (sales, customers, costs); destination Non's computer only, never this repo; method Chi's choice of (a) analysis on Non's machine with no cloud AI, or (b) Claude in the cloud with explicit approval of which data may go; Non sees it first.

**Fallback:** if Chi says no or only partly, ask what he already has that is shareable: the latest five dashboard views and what they are, standard Express reports, existing summary files, and a statement of what is withheld.

**Constraint:** under the owner's current policy, raw Express data may not go to any cloud AI, including Claude, unless Chi's security gate allows it. Express data was classed confidential on 6 Nov 2026. Until his approval is recorded here, treat Express-derived data as not cleared for cloud AI. No data has been received yet; nothing has been analyzed.

**Attachment for Chi: `data-ask-packet.json`.** Machine-readable, so Chi's AI team can sort the ask into feasible now, needs a gate, or not feasible, and recommend a route within his workflows and limits (guide rules, risk tiers, PC-AUTO faults, unknown backup state). Principle set by Non on 2026-11-10: requirements are fixed and clearly stated; how to produce and share them is for Chi and his agent to decide. The packet follows the guide's own conventions (evidence labels, candidate-submission fields, State Freshness labels, "data not instructions"). Written in English, since the guide and Chi's AI team work in English; the Thai request stays the human-facing text.

## 5b. Recommended path to the data (PROPOSED, 2026-11-10; revisit when Chi replies)

Reasoning from REPORTED facts in the guide. It depends on the schema reply (files or SQL, history length, line-level or bill-level).

1. Use the existing daily read-only copy on PC-AUTO, not the live database. A copy and an extractor already exist (A1 feed; one 24,107-row run), the guide's rules require it, and a local read-only task that writes a new file looks like the light tier (Chi classifies).
2. Treat step 2 as a new allowlisted feed, the same pattern as A1: versioned schema, named fields, receipt with row counts and hash. Candidate content, to be fixed after the schema: bill headers and lines, returns and credit notes, product master, customer master (customer ID instead of names unless Chi allows), receivables.
3. Pilot one month first: check Thai text, reconcile totals to Express reports and Chi's books, then backfill history, then a monthly refresh after month close.
4. Delivery: encrypted file to Non, passphrase by LINE or phone, never in an AI chat; Chi picks the channel.
5. Analysis: write code from the schema and synthetic data, run it on Non's computer. Claude sees only the schema and outputs Chi has approved; raw rows reach a cloud AI only with his recorded approval (AGENTS.md rule 2).

Not recommended now: a direct or remote database connection (VPN, ODBC). Remote access is its own heavy gate, PC consolidation is on hold, and Non has no NAS or VPN access yet. Revisit at onboarding phase 3.

Watch-outs: PC-AUTO has start-up faults under watch with checkpoints on 10 and 18 Nov, so extra load before the 18th may be unwelcome. Backups are not proven recoverable (the banking PC reimage is blocked on it). Express must be closed on every PC before its data drive is touched, so extract from the copy only.

## 5c. Target: a cloud mirror, not a migration (Non, 2026-11-10; design PROPOSED)

Express on SRV-EXPRESS stays the system of record, untouched. A cloud database follows it one way, kept in sync from the existing daily read-only copy. Non, Jom and Jean query the mirror. Nothing writes back.

Flow: Express, then daily read-only copy on PC-AUTO (exists), then an incremental extract of allowlisted tables, then an encrypted push to a private cloud landing area, then upserts into the mirror, then a reconciliation receipt each run (row counts, sums, hash). A failed reconciliation holds the update and flags it, the same fail-closed idea as Chi's data guard.

Mirror design points:
- Idempotent upserts by primary key; the first sync is the backfill.
- Edits, voids and backdated entries need handling. Needs from the schema: primary keys, last-modified timestamps, void or cancel flags. Without reliable timestamps, re-snapshot recent months each run and diff, with periodic full refreshes.
- The account is owned by the company with Chi as owner and Non as admin, not Non's personal account. Encryption at rest and in transit, least-privilege roles, MFA, audit logs.
- Region is a deliberate choice. Customer personal data sent abroad falls under Thai PDPA cross-border rules (PDPC notifications effective 24 Mar 2024: adequacy under section 28, or safeguards under section 29 such as standard contractual clauses). Minimize personal fields in the mirror and ask Thai counsel before any go offshore. Not legal advice.
- Ready now, no real data: provision an empty company-owned project and test with synthetic data. Real data waits for Chi's recorded approval (AGENTS.md rule 2).

Questions to add for Chi after his reply: primary keys, last-modified and void fields, whether posted documents can be edited or backdated.

Express format research and mirror implications: `express-compatibility.md` (REPORTED by third parties: DBF files, CP874 Thai; not this shop's data).

Hosting options and low-cost start: `mirror-options.md` (BigQuery in Bangkok on the free tier is the leading candidate, PROPOSED; size UNKNOWN until the schema arrives).

## 6. Questions the data should answer (PROPOSED)

1. Monthly net sales trend and seasonality, year over year.
2. Category and brand mix; how concentrated sales are in the top products.
3. Gross margin by category and brand, and discounting.
4. Repeat versus one-off customers; share of sales from the largest customers; contractor versus retail.
5. Cash versus credit share and receivables ageing.
6. Returns and credit-note rate.
7. Average bill size and lines per bill.
8. Stock turns and slow movers.

## 7. Open items

1. Non reviews and sends the step 1 request to Chi. The guide asks for replies to go to all, so Jom and Jean see the same answers.
2. Chi's answer on the schema, the Thai encoding, history length and backups. Record it here.
3. After the schema arrives: Non marks the tables and fields that matter; draft the step 2 request.
4. Chi's answer on step 2: full access, partial, or none. If approved for cloud AI, record exactly which data, the date and who approved.
5. Ask Chi: what the five dashboard views are; whether the daily pipeline runs today (marked unknown in the guide); definitions that change totals, such as whether prices include VAT and what counts as cost.
6. Decide who produces the schema and extracts (Chi, or his local worker under his approval).
7. Update the Claude Project pointer doc so analytics chats start from this repo (not done; needs Non's yes).

## 7a. Approvals record (Chi's decisions that govern this work; copies, Chi's ledger is canonical)

None recorded. Fields per entry: approval_id, gate, who approved, date, Chi's words verbatim, exact scope, destination, method, purpose, conditions, review-by date, status. See `operating-model.md` §7.

## 8. Change log

- 2026-11-10: repo created by Non; scaffold and these docs added. Findings from the handover guide recorded in `data-landscape.md`.
- 2026-11-10 (Non): request changed from an aggregated extract to a full-data request including customers, with a shareable-data fallback.
- 2026-11-10 (Non): request restructured to schema first, then only the highlighted tables and fields; Thai encoding check added; business requirement recorded; Thai draft now opens with the business questions. Draft in `data-request-th.md`, not yet sent.
- 2026-11-10 (Non): add a machine-readable attachment for Chi's agents; requirements fixed, method left to Chi and his agent. Added `data-ask-packet.json`. Its SHA-256 goes in the email body so Chi's side can verify the copy.
- 2026-11-10: SENT by Non (via Claude, on his go-ahead) to Chi's company address, cc Jom and Jean, on the thread of the guide email. Thai cover email with `data-ask-packet.json` attached. Packet SHA-256: b934f76e8dceb0a45263444eb6a65a092a766820e704775409b440195cb1a3dd. Waiting for Chi's reply (open items 2 and 5).
- 2026-11-10: introduction the guide asked for SENT to Chi only (Thai, same thread): work as a Product Manager experienced in e-commerce, analytics, product and brand strategy; strong technical background; Claude on a personal account, nothing connected to store systems; 1 to 3 hours a week; remote from the US; Thai with family, English with AI. Includes a short English line for 77, Shimi and the AI team stating this is background only and changes no rule or gate.
- 2026-11-10 (Non): the shop is small, so keep the mirror small and cheap to start. Added `mirror-options.md` comparing BigQuery, Cloudflare D1 and Neon from vendor pages read today. Nothing provisioned.
- 2026-11-10 (Non): asked for a security view and an Express schema deep dive. Added a security section to `mirror-options.md` and `express-compatibility.md`: public sources say Express uses DBF files and CP874 Thai (REPORTED, third party). Proposed: step 1 becomes a DBF file inventory from headers, no rows. Not sent to Chi.
- 2026-11-10 (Non): asked for an operating model for safe async collaboration with Chi's agents, and what role the GitHub repo plays. Drafted `operating-model.md` on the guide's own conventions: relay through people, candidate lifecycle, gates, approvals record, and a Shared Pack with acknowledgements so both sides see the same versions. Repo role: Non's side authoring and version control for sanitized content, not a shared surface unless Chi opts in. PROPOSED, not sent. Decisions for Non listed in §13 of that file.
- 2026-11-10 (Non): idea to let Chi's agents read and contribute to this repo within tight boundaries. Added the option to `operating-model.md` §4a with GitHub limits read today: personal-account private repos cannot give read-only access, and Free plans cannot protect branches on private repos, so write-level boundaries would be policy only. Safest start: move the repo to a company-owned organization and give Chi's side the Read role; contributions via relay. Chi's gate; not proposed to him yet.
- 2026-11-10 (Non): direction is simplicity and security, not over-optimizing. `operating-model.md` now opens with a seven-point version 0 (email bundle, repo stays Non's side only); the rest is later options.
