# Sales analytics: project plan and source of truth

Read this first in every new chat, after `AGENTS.md`. Last updated: 2026-11-10. Keep dated notes short.

## 1. Goal

Understand the business performance of นนท์วัสดุ (Nonwatsadu Plus Co., Ltd.) from its sales data, urgently. Non wants full access to the data, customer data included, and wants the request to Chi (Non's dad, the owner) sent as soon as possible. Digital presence work continues separately in `nonwachi/nonwatsadu-website`.

## 2. Business context (short; REPORTED by Non, see the website repo plan)

- Family building-materials shop in Mueang Lampang, Thailand. About 80% of customers are contractors and tradespeople; paint draws homeowners.
- Products: paint, lumber and boards, furniture-making supplies, steel, cement. Offers vetted credit and delivery. No cutting service.
- Non runs this from the US; Chi runs the shop and the shop's systems.

## 3. Sources

- NONT PLUS Successor Handover & Onboarding Guide, Joint R1d, 10 Nov 2026, written by Chi's AI team (77 = Claude, Shimi = ChatGPT) and emailed to Non, Jom and Jean. Not stored here: Chi approves each recipient's copy. Findings from it are in `data-landscape.md`.
- Web check, 10 Nov 2026: public sources did not show how Express stores or exports data. UNKNOWN until Chi answers.

## 4. Decisions

- 2026-11-10 (Non): analytics work gets its own repo, not the website repo.
- 2026-11-10 (Non): the data request goes to Chi as soon as possible.
- 2026-11-10 (Non): the request asks for all the data, customer data included. If Chi cannot give that, it asks what he already has that is shareable.

## 5. The request (draft: `data-request-th.md`)

**Primary ask:** a read-only copy of everything in Express relevant to sales and profit, as far back as it goes: sales at line-item level, customers with names, returns and credit notes, products, stock and cost, purchases.

**Handling terms, proposed in the draft for Chi to rule on** (the guide's gate asks for data class, destination, method and purpose):
- Class: sales, customers, costs; confidential.
- Destination: Non's computer only; never in this repo.
- Method, Chi chooses: (a) analysis on Non's machine with no cloud AI, or (b) analysis with Claude in the cloud, which needs Chi's explicit approval of which data may go.
- Who sees it: Non first.

**Fallback if Chi says no or only partly:** ask what he already has that is shareable: the latest five dashboard views and a list of what they are, standard Express reports (sales by product, customer, month), existing summary files, the same data with customer codes instead of names, and a statement of what is withheld.

**Constraint to keep in view:** under the owner's current policy, raw Express data may not go to any cloud AI, including Claude, unless Chi's security gate allows it. Express data was classed confidential on 6 Nov 2026. Until his approval is recorded below, treat Express-derived data as not cleared for cloud AI. No data has been received yet; nothing has been analyzed.

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

1. Non reviews and sends the request to Chi. The guide asks for replies to go to all, so Jom and Jean see the same answers.
2. Chi's answer: full access, partial, or none. Record it here with date and scope.
3. If approved for cloud AI: record exactly which data, the date, and who approved.
4. Ask Chi: history length; fields in the sales records; what the five dashboard views are; whether the daily pipeline runs today (marked unknown in the guide).
5. Ask Chi: how Express stores data and which reports export to Excel or CSV; definitions that change totals, such as whether prices include VAT and what counts as cost.
6. Decide who produces the extract (Chi, or his local worker under his approval).
7. Update the Claude Project pointer doc so analytics chats start from this repo (not done; needs Non's yes).

## 8. Change log

- 2026-11-10: repo created by Non; scaffold and these docs added. Findings from the handover guide recorded in `data-landscape.md`.
- 2026-11-10 (Non): request changed from an aggregated extract to a full-data request including customers, with a shareable-data fallback. Draft in `data-request-th.md`, not yet sent.
