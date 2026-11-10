# Cloud mirror: hosting options and a low-cost start

Written 2026-11-10 for `PROJECT-PLAN.md` §5c. Hosting facts below were read from the vendors' own pages on 2026-11-10 and can change; re-check before provisioning. Data size is UNKNOWN until Chi sends the schema, row counts and date ranges. Nothing here is a decision. Chi approves any real data going anywhere (`AGENTS.md` rule 2).

## Constraint from Non (2026-11-10)

Small operation, so the database should be small. Find a path with little or no start-up cost. Provision something only when there is a clear path for the one-way mirror.

## Options compared

| | Google BigQuery | Cloudflare D1 | Neon (managed Postgres) |
|---|---|---|---|
| Free allowance | 10 GiB storage and 1 TiB of queries per month | Free plan: 500 MB per database, 5 GB per account, 10 databases | Free plan: 1 GB per project, 100 compute-hours per month |
| Beyond free | On-demand queries $6.25 per TiB; active storage billed per GiB-hour (about 50% less after 90 days untouched) | Paid plan raises the per-database cap to 10 GB, and the page says that cannot be raised further | Paid plan (not read) |
| Region nearest Thailand | Bangkok (`asia-southeast3`), also Singapore and Jakarta | Not read; not checked | Singapore (`aws-ap-southeast-1`) and Sydney in Asia Pacific. No Thailand region |
| Query style | Full analytic SQL, built for scans over years of data | SQLite. Queries stop at 30 seconds; results over 50 MB fail (from 4 Nov 2026) | Full Postgres |
| Limits that matter here | Free tier needs checking for billing-account requirements (not read) | 500 MB free cap could be tight; each database runs one query at a time | Free compute scales to zero after 5 minutes and cannot be turned off. Restore history only 6 hours. If limits are hit, compute suspends; data is not deleted |
| Fit for a one-way mirror | Good. Load files in batches, upsert with a merge, query from anywhere | Workable for small data. Already in use for the website tap counter | Good. Familiar tooling, but free plan is thin for a business mirror |

## Reading of the options (PROPOSED)

- **BigQuery first.** The free allowance is large relative to a small shop, there is a Bangkok region so data can sit in Thailand, and the work is analysis (scans, joins over years), which is what it is built for. Start-up cost is expected to be near zero. Still to check before relying on it: what the free tier requires (a billing account may be needed even if usage stays free) and how Bangkok compares with Singapore on features.
- **D1 second.** Cheapest to reach because the Cloudflare account exists, but it is SQLite with a 500 MB free cap and query limits. It becomes a reasonable choice only if the schema reply shows a very small database. The website account is Non's, and the mirror should sit in a company-owned account (`PROJECT-PLAN.md` §5c).
- **Neon last.** Good Postgres, but the free plan is small, sleeps, has a 6-hour restore window, and there is no Thailand region. A paid plan would be needed for a real mirror.

## Region and PDPA

Storing in Bangkok keeps the stored copy in Thailand. That does not by itself settle access from the US or any personal data sent abroad. Ask Thai counsel before real customer data is loaded. Minimize personal fields in the mirror. Not legal advice. Background: [Tilleke & Gibbins on PDPC cross-border rules](https://www.tilleke.com/insights/thailand-unveils-regulations-for-cross-border-personal-data-transfer).

## Size check before provisioning

Free tiers only help if the data fits. Chi's schema reply gives row counts and date ranges per table. Rough rule: if the allowlisted tables total well under 1 GB, every option above starts free. The 24,107-row pricing extract on 12 Sep 2026 is not a sales table and says nothing about sales history size.

## Next steps

1. Wait for Chi's schema reply (size, keys, last-modified and void fields).
2. If it is small, create an empty BigQuery dataset in a company-owned Google project, in Bangkok, with synthetic data only. Cost expected near zero; confirm the free-tier terms first.
3. Write the load and reconciliation code against synthetic data in `analysis/`.
4. Load real data only after Chi's approval is recorded in `PROJECT-PLAN.md`.

## Sources (read 2026-11-10)

- [Cloudflare D1 limits](https://developers.cloudflare.com/d1/platform/limits)
- [BigQuery locations](https://docs.cloud.google.com/bigquery/docs/locations)
- [BigQuery pricing](https://cloud.google.com/bigquery/pricing)
- [Neon regions](https://neon.com/docs/introduction/regions)
- [Neon free plan limits](https://neon.com/faqs/free-plan-limits-and-quotas)
