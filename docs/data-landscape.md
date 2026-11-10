# Data landscape: what exists, how granular, how to reach it

As of 2026-11-10, from the handover guide (Joint R1d). Nothing here was checked against the live systems. The guide itself says today's state of most of this is unknown; re-check before relying on it.

Evidence labels: VERIFIED-DATED (checked on a stated date, in the named author's records), REPORTED (stated, not checked), PROPOSED, UNKNOWN. See `AGENTS.md`.

## Layers

| Layer | What it is | Granularity | Label and source |
|---|---|---|---|
| Express (SRV-EXPRESS) | Thai accounting program; records every sale, purchase and stock movement. Staff PCs use the Express client | Not stated; "every sale" suggests transaction level (inference) | REPORTED, guide §1, §7 |
| Daily read-only copy (PC-AUTO) | Early-morning job, or "Refresh" in the NWP Hub launcher, copies Express data read-only to the automation PC | Same as Express, presumably | VERIFIED-DATED Jul 2026 in 77's records; running today UNKNOWN |
| Owner and staff search pages | Built from the daily copy | Not stated | VERIFIED-DATED Jul 2026 in 77's records |
| Five-view dashboard | For owners; reconciled to zero difference (against what is not stated) | Not stated. The five views are not named | VERIFIED-DATED Jul 2026 in 77's records; not inspected by Shimi |
| A1 product feed | Versioned schema, 10 allowlisted source-fact fields, product facts only, no price suggestions or margin decisions. Published after the data guard passes | Product level. Looks like product facts, not sales (inference) | REPORTED Sep 2026; field names are in the contract file, not the guide |
| Pricing workbook | Owner-governed Excel model; one extractor run passed 24,107 rows on 12 Sep 2026 | What a row is, is not stated | VERIFIED-DATED for that run only, in Shimi's records |
| Owner cockpit | One local view of repeat customers, products, cost, margin and competitor prices; morning report on Chi's phone over the private VPN | n/a | PROPOSED, not built |

## What the fields probably include (unconfirmed)

The guide's cockpit plan and its VAT lesson ("check whether a price includes VAT before trusting a margin") imply Express holds customer, product, cost and price. No field list is given.

## Not stated anywhere in the guide

- How many years of history exist, and the date range
- Whether records are invoice level or line-item level
- Salesperson, branch, credit and receivables, or returns fields
- Whether Express stores data as files or in a SQL database, and what its export options are
- Where the daily copy sits and in what format

## How to reach the data

- **Only Chi can.** Non has no access. NAS accounts, VPN and connectors are not part of the invitation; remote access is its own gate. Early phases run on email and files only (guide §10).
- **Rules that shape any request** (guide §5, rules 2, 5, 7, 11):
  - Raw Express data never goes to a cloud AI unless the data class, destination, method, purpose and an Owner security gate all allow it. Sanitized summaries may be discussed with cloud AIs.
  - Express data was classed confidential on 6 Nov 2026 (REPORTED).
  - Express must be closed on every PC before anything touches its data drive. Automation works on the read-only copy only.
  - A local, read-only task that writes a new file looks like the guide's "light" tier (self-test, one Owner run). That is Chi's call.
- **Local AI:** Ollama is installed on the automation PC but not in use (REPORTED, Nov 2026). It is the route for analysis of raw data, if Chi ever allows it.
- **Routes**, as Non decided on 2026-11-10: ask Chi for everything, customers included, with his chosen handling (local analysis, or cloud AI with explicit approval). Fallback: what he already has that is shareable, such as the dashboard views, standard Express reports, existing summaries, or the data with customer codes. Detail in `PROJECT-PLAN.md` §5.
