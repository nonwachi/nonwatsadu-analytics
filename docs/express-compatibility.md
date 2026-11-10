# Express schema: what public sources show, and what it means for the mirror

Written 2026-11-10. Every Express fact below comes from public forum posts and one third-party open-source app, not from Express documentation and not from the shop's own system. Label for all of it: REPORTED (third party), not checked against the shop's data. The shop's Express version and files are UNKNOWN until Chi replies. Nothing here is Express's official schema.

## What the sources say

| Finding | Source |
|---|---|
| Express stores data as DBF files, the dBASE/FoxPro file structure. Express itself is not written in FoxPro; it only uses the DBF format. Visual FoxPro 6 to 9 can open the files | [Express community forum, thread on how Express is built](https://esg.co.th/phpBB3/viewtopic.php?t=15388&p=54915); [forum thread on versions](https://esg.co.th/phpBB3/viewtopic.php?t=12347&p=45570) |
| Express-made DBF files failed in the generic dBase ODBC driver ("External table is not in the expected format") and opened with the Visual FoxPro ODBC or OLE DB driver. Thai text displayed wrongly through that driver | [ThaiCreate forum thread](https://www.thaicreate.com/php/forum/136631.html) |
| Reading the live files over a network share failed or was discouraged. Suggested instead: copy the files and read the copy | same thread |
| An open-source web app reads Express DBF files read-only over CIFS and decodes Thai as CP874 (Windows-874), trimming byte 0xA0 | [papound/my-express README](https://github.com/papound/my-express) |
| Files named: `ARMAS` (customers incl. credit limit and debtor history), `ARTRN` (tax invoices and receipts), `OESO` and `OESOIT` (quotations and their lines, "IT" presumed lines), `ARBIL` and `ARBILIT` (billing documents), `ARRCPIT` (receipts; fields RCPNUM, DOCNUM), `STMAS` (stock master), `ISTAB` (lookup codes; fields tabtyp, typcod), `ISINFO` (company info) | the two sources above |
| `STMAS` fields seen: stkcod, stkdes, stktyp, stkgrp, totbal, lasupd, credat, chgdat | ThaiCreate thread |
| Newer versions added fields under Revenue Department announcements 194 to 197 (tax ID, branch code, BILLTO, BILLBE). The old version line (RG4) is no longer developed | forum thread on versions |
| The only export found in public sources is "Export to Excel" on reports that offer it. No CSV or text export was mentioned | [LNL Biz tip page](https://www.lnlbiz.co.th/express-tips/express-export-to-excel) |

## What it means

1. **No live database to connect to.** If the shop runs a DBF version, there is no SQL server to replicate from. The mirror is a file-to-warehouse job: read the DBF files from the read-only copy, decode, write UTF-8, load. Chi's existing extractor already reads Express data from that copy, so the first step is probably already in his tooling (REPORTED in the guide).
2. **The warehouse choice barely depends on Express.** BigQuery, D1 and Postgres all load plain UTF-8 CSV or Parquet. BigQuery does not read DBF, so conversion sits in the extract step, which is also where Thai gets fixed.
3. **Thai text.** Decode DBF text as CP874 in our own code and write UTF-8. Do not trust Thai shown through a generic ODBC driver. Test on the 20-name product sample requested in step 1. BigQuery CSV loads: my understanding is that only UTF-8 and ISO-8859-1 are accepted; verify in Google's docs before the first load.
4. **Incremental sync is only partly possible.** `STMAS` carries created and changed dates (credat, chgdat, lasupd). Whether the sales tables do is UNKNOWN. Without change dates, re-snapshot recent months each run and diff, as planned in `PROJECT-PLAN.md` §5c.
5. **Deleted and edited records.** General DBF background, not from the sources above: a DBF delete normally only flags the record, and the row disappears when the file is packed, so a mirror must treat "gone from the file" and "flagged deleted" explicitly. Posted documents may also be edited or backdated. Ask Chi (already on the follow-up list).
6. **Schema drift across versions.** Fields were added by Revenue Department changes. Version the schema, record the Express version in every load, and have the loader fail on unknown or missing fields instead of guessing.
7. **Quotations are not sales.** `OESO` and `OESOIT` are quotations. Sales lines, returns, credit notes and cost fields were not found in public sources and their file names should not be guessed. Chi's file inventory settles it.
8. **Field types.** General DBF background: fixed-width text is space-padded and must be trimmed, field names are short (up to 10 characters), and money is stored as decimal numbers. Check VAT and cost meaning per field (`AGENTS.md` rule 7).

## Sharper step 1 ask (PROPOSED)

For a DBF shop the schema is readable from the file headers without opening any rows. Ask Chi's side for a file inventory: every DBF, memo (FPT) and index (CDX) file with record count, last-modified date, and the field list (name, type, length, decimals) read from the header. A short read-only script can produce this, and Non can write it for Chi's team to review and run on the read-only copy. It outputs structure only, no rows. Not built yet; needs Non's yes.

## Still unknown

Express version and whether this shop uses DBF; which files hold sales lines, returns and cost; which have change dates; whether deleted records are packed; history length; Thai storage in this shop's files.
