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

## Express file format (REPORTED by third parties, 2026-11-10)

Public forum posts and an open-source app indicate Express stores data as DBF files (FoxPro-style) and that Thai text is CP874. Not from the guide, not checked for this shop. Details and implications: `express-compatibility.md`.

## Not stated anywhere in the guide

- How many years of history exist, and the date range
- Whether records are invoice level or line-item level
- Salesperson, branch, credit and receivables, or returns fields
- Whether Express stores data as files or in a SQL database, and what its export options are
- Where the daily copy sits and in what format

## Role of the main NAS (NAS-MAIN)

REPORTED (guide §6, §7). The shared shelf for finished work and team knowledge, not the origin of sales data.

- AI Work Exchange (AWE): where the A1 product feed lands, with a receipt, for the pricing work
- Knowledge Hub: canonical team knowledge; authority moved to the NAS 30 Sep 2026; changes only through a publisher script Chi runs
- Filed documents from the OCR pipeline (bank and finance PDFs), git repositories, shared files, the notification server
- Chi runs all NAS writes himself; the coding helper (cc) is blocked from NAS paths by a hook (enforcement today UNKNOWN)

UNKNOWN: whether the daily Express copy or dashboard output is stored on the NAS or only on the automation PC; where Express backups live; the share and permission map. Asked in `data-request-th.md` §4.

## Thai text and encoding

UNKNOWN for Express: the guide says nothing about it, and public sources found on 10 Nov 2026 did not describe Express's storage format.

General background, not from the guide and not confirmed for Express: older Thai Windows software often stores text in a legacy Thai code page (TIS-620, ISO 8859-11 or Windows-874) instead of Unicode (UTF-8). Such text shows as garbled characters when a tool assumes the wrong encoding. That is usually fixable once the right code page is known. It is not fixable if an earlier step already replaced Thai characters with "?". Reference pages on these encodings: [Windows-874 mapping (encoding_rs)](https://docs.rs/crate/encoding_rs/0.8.6/source/doc/windows-874.txt), [ISO 8859-11 decoding guide](https://mojoauth.com/dev-guides/iso-8859-11-encoding--go.md).

Plan: ask Chi how Thai is stored, and for about 20 product names (not customer data) as a test sample. Decode them with the candidate encodings and compare against what Express shows on screen before any large extract. Ask for extracts as UTF-8 (CSV for Excel needs UTF-8 with a byte-order mark to show Thai correctly).

## Schema first

Decided 2026-11-10 (Non): ask Chi for the full data schema before any data. Names, types, relationships, row counts and date ranges, no rows. Non then marks the tables and fields that matter, and the data request covers only those. A schema describes structure, not business data; Chi confirms it is fine to share.

## How to reach the data

- **Only Chi can.** Non has no access. NAS accounts, VPN and connectors are not part of the invitation; remote access is its own gate. Early phases run on email and files only (guide §10).
- **Rules that shape any request** (guide §5, rules 2, 5, 7, 11):
  - Raw Express data never goes to a cloud AI unless the data class, destination, method, purpose and an Owner security gate all allow it. Sanitized summaries may be discussed with cloud AIs.
  - Express data was classed confidential on 6 Nov 2026 (REPORTED).
  - Express must be closed on every PC before anything touches its data drive. Automation works on the read-only copy only.
  - A local, read-only task that writes a new file looks like the guide's "light" tier (self-test, one Owner run). That is Chi's call.
- **Local AI:** Ollama is installed on the automation PC but not in use (REPORTED, Nov 2026). It is the route for analysis of raw data, if Chi ever allows it.
- **Routes**, as Non decided on 2026-11-10: ask Chi for everything, customers included, with his chosen handling (local analysis, or cloud AI with explicit approval). Fallback: what he already has that is shareable, such as the dashboard views, standard Express reports, existing summaries, or the data with customer codes. Detail in `PROJECT-PLAN.md` §5.
