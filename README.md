# Meridian Estates — Synthetic Training Data Pack

**For:** Building Real Estate Intelligence with Generative AI, n8n, Claude & Claude Code
**Version:** v1.0 · generated 15-Oct-2026 · as-of date used throughout: **14-Oct-2026**

> **EVERYTHING IN THIS PACK IS FICTIONAL.** Meridian Estates Pvt Ltd, its three projects, every
> buyer, document, registration number, rate, deadline and financial figure are invented for
> training. No figure describes any real organisation's performance. Nothing here is legal, tax,
> regulatory or financial advice. Registration numbers are written in a realistic format and must
> never be reused in a real document.

---

## 1. What is in the pack

### Source documents (Markdown — the "approved material" for grounded builds)

| File | Used in | Contains |
|---|---|---|
| `01-brochure-and-faq.md` | Build 2, Build 10, Demo 22 | Approved project info v2026-09: brochure pages, 12 FAQs, and an explicit list of what it does *not* state |
| `02-rera-rules-excerpt.md` | Build 1 | Plain-language paraphrase of 8 provisions, 6 answerable and 6 unanswerable test questions |
| `03-sale-agreement-extract.md` | Demo 3, slide 17, Demo 8 | Agreement for unit VH-B-1204, Schedule II, cost sheet, and two deliberate gaps |
| `04-project-registration-sheets.md` | Demo 11, Demo 22 | Registration sheets for all three projects — the verifier's source of truth |
| `16-clause-comparison.md` | Demo 16 | Two versions of a possession clause, four differences, one an omission |
| `17-due-diligence-pack.md` | Build 13 | 12-item checklist, 12 document extracts, 3 planted defects + 1 inference |
| `18-marketing-copy-drafts.md` | Demo 22 | Four copy drafts: three with planted unsupported claims, one clean |
| `21-allotment-letter-template.md` | Demo 19 | Allotment letter and demand notice templates, placeholder rules, 7-step pipeline |

### Datasets (CSV / JSON)

| File | Rows | Used in | Notes |
|---|---|---|---|
| `05-compliance-register.csv` | 18 | Build 5, Demo 11 | Obligations with **owner-entered** due dates and a `due_date_source`. Two rows have no due date — deliberate data gaps |
| `06-unit-inventory.csv` | 480 | Build 9, Build 17, Demo 18 | Full unit master: areas, rates, floor rise, PLC, discount, consideration, status, buyer ref, money to date |
| `07-price-list.csv` | 2 | Build 17 | Published ranges and rate basis per configuration |
| `07-rates.json` | — | **Build 17** | The dated rate table the calculator reads. Never hard-code a rate in the code |
| `08-enquiry-log.csv` | 1,200 | Build 4, ROI section | One month of enquiries across all projects. Includes duplicates, missing emails, no-consent rows, night arrivals |
| `09-sample-leads.json` | 5 | **Build 4** | The five webhook payloads LD-4117 … LD-4121 |
| `10-bias-test-pairs.csv` | 20 (10 pairs) | **Demo 21** | Identical business attributes; only name, locality and language vary |
| `11-site-visit-log.csv` | 154 | Demo 7, Build 9 | Visits with attendance, objection field, competitor mention, next step |
| `12-collections-ledger.csv` | 212 | Build 9, Demo 8, Demo 14 | Per booked unit: demanded, collected, outstanding, ageing bucket |
| `12b-demand-ledger.csv` | 994 | Demo 8, Demo 14 | One row per milestone demand, with certification date and withheld-demand notes |
| `13-payment-schedule-VH-B-1204.csv` | 8 | **Demo 8**, slide 17 | The hero unit's schedule, matching the agreement extract to the rupee |
| `14-complaints.csv` | 40 | **Demo 6** | Category, rule-based priority, keyword override, route, SLA, legal flag |
| `15-micro-market.csv` | 5 | **Demo 15** | Competitor set with **source and source date on every row** |
| `19-dashboard-sample.json` | — | **Build 9** | The dashboard feed in the slide-52 response shape, with field definitions |
| `20-mis-monthly.csv` | 7 | **Demo 14** | Bookings vs target, visits, conversion by month, Apr–Oct |
| `22-document-merge-data.csv` | 25 | **Demo 19** | Merge rows; four rows are BLOCKED for an incomplete address |
| `23-scoring-and-routing-rules.json` | — | **Build 4**, Demo 21 | Point rules, bands, routing table, **excluded attributes**, and the expected score for each sample lead |
| `meridian-synthetic-data.xlsx` | — | everything | All tabular files as one workbook, one sheet per dataset |

---

## 2. The numbers tie to the deck

Generated and asserted, not estimated. If you change the data, re-check these.

| Figure | Value | Appears on |
|---|---|---|
| Total units | 480 (booked 212, on hold 25, available 243) | Slides 53, 54, 57 |
| Average realisation | **Rs 8,940 / sq ft** exactly | Slide 53 |
| Amount demanded to date | **Rs 182.40 Cr** exactly | Slides 53, 54, 65 |
| Amount collected | **Rs 164.10 Cr** exactly | Slides 53, 54, 65 |
| Outstanding | **Rs 18.30 Cr**, of which **Rs 4.70 Cr** over 90 days | Slides 53, 54, 65 |
| Collection efficiency | **89.97%** | Slides 53, 54 |
| Bookings by month | Apr 14, May 17, Jun 21, Jul 19, Aug 26, Sep 24, Oct 22 (MTD) | Slides 54, 65 |
| Verdant Heights funnel, 30 days | 318 → 246 → 112 → 84 → 37 → 22 | Slide 54 |
| Site visits attended | 84 in September; 84 in the trailing 30 days | Slides 54, 65 |
| Visit-to-booking | 26.19% (30-day), 28.6% (September) | Slides 54, 65 |
| Unit VH-B-1204 consideration | **Rs 1,51,63,200** | Slides 26, 47, 71, 72 |
| VH-B-1204 total payable | Rs 1,73,38,552 | Slide 72 |
| VH-B-1204 paid to date | Rs 68,23,440 (3 of 8 milestones) | Slide 47 |
| Enquiries per month, all projects | 1,200 | ROI section |

**How the collections tie:** each tower has one construction stage (A and B at Slab 5 / 60%,
C at Slab 10 / 75%, D at Flooring / 95%), so each booked unit's demanded-to-date is the sum of its
certified milestones. Two units carry a withheld or part-raised demand marked
*"pending reconciliation"* in `12b-demand-ledger.csv` — that is realistic and it is also what makes
the portfolio total land exactly on Rs 182.40 Cr.

### One deliberate definition clash — use it

Slide 54 shows portfolio **outstanding** as demanded − collected (Rs 18.30 Cr), while the slide-52
unit row shows `outstanding_inr` of about Rs 83.4 lakh for VH-B-1204, which is the **balance
consideration** (total − collected). Both are legitimate; they are different metrics with the same
name. This pack therefore ships two explicitly named columns:

- `demand_outstanding_inr` — demanded minus collected (**Rs 22,74,480** for VH-B-1204)
- `balance_consideration_inr` — consideration minus collected (**Rs 83,39,760** for VH-B-1204)

Use the clash in Module 3: it is the metric-definition argument from slide 51, in live data.

---

## 3. Suggested use, module by module

**Module 1** — attach `02-rera-rules-excerpt.md` to the RERA Desk project for Build 1; attach
`01-brochure-and-faq.md` for Build 2; use `03-sale-agreement-extract.md` for Demo 3. Each file ends
with the questions it cannot answer, so you can test the refusal in thirty seconds.

**Module 2** — post rows from `09-sample-leads.json` to the Build 4 webhook. `23-scoring-and-routing-rules.json`
carries the expected score and route for each, so participants can check their workflow against an
answer key. `08-enquiry-log.csv` is the volume test (50 leads in a minute) and the ROI baseline.
`05-compliance-register.csv` drives Build 5; `14-complaints.csv` drives Demo 6; `11-site-visit-log.csv`
drives Demo 7; `13-payment-schedule-VH-B-1204.csv` drives Demo 8.

**Module 3** — serve `19-dashboard-sample.json` as the response for Build 9, or compute it live from
`06-unit-inventory.csv` + `12-collections-ledger.csv` and compare. `01-brochure-and-faq.md` and
`04-project-registration-sheets.md` are the content for Build 10's microsite.

**Module 4** — `17-due-diligence-pack.md` for Build 13; `20-mis-monthly.csv` +
`12-collections-ledger.csv` for Demo 14; `15-micro-market.csv` for Demo 15;
`16-clause-comparison.md` for Demo 16.

**Module 5** — `07-rates.json` is the configuration file Build 17 must read. `06-unit-inventory.csv`
gives the test units: VH-B-1204 (floor 12, park facing) for the happy path, any `*-0501` unit for
the floor-5 boundary, any `*-0601` for the first charged floor. `22-document-merge-data.csv` and
`21-allotment-letter-template.md` drive Demo 19.

**Module 6** — `10-bias-test-pairs.csv` for Demo 21 (run all 20 rows; every pair must score
identically). `18-marketing-copy-drafts.md` + `04-project-registration-sheets.md` for Demo 22.

---

## 4. Planted defects — the answer key

Keep this section away from participants until after each exercise.

| Where | Defect |
|---|---|
| `08-enquiry-log.csv` / sample leads | LD-4120 duplicates LD-4117 (same phone, different spelling); LD-4121 has no marketing consent; LD-4118 wants possession before Dec-2026 against a published Mar-2027; LD-4117 has no email and arrives at 23:41 |
| `08-enquiry-log.csv` | ~7% duplicate rows; ~22% blank emails; some rows have no first reply at all |
| `05-compliance-register.csv` | Two obligations with no due date (data gaps); one overdue water and sewerage approval; one at-risk draft-only update |
| `17-due-diligence-pack.md` | EC gap 2004–2007; survey number 112/3 vs 112/3A; one of four heirs' release deed missing; extent not cross-verifiable; two approvals absent |
| `03-sale-agreement-extract.md` | 12% interest binds the allottee only — no promoter-delay remedy; maintenance terms absent |
| `16-clause-comparison.md` | "shall" → "shall endeavour"; 6 → 12 month grace; enumerated force majeure → open-ended; clause 6.7 deleted |
| `18-marketing-copy-drafts.md` | Draft 1: 7 unsupported factual claims incl. an assured-return representation (escalate, do not correct). Drafts 2 and 3 carry more |
| `22-document-merge-data.csv` | Four rows BLOCKED for an incomplete address — generation must stop for those, not proceed |
| `12b-demand-ledger.csv` | Two demands withheld/part-raised pending reconciliation |
| `14-complaints.csv` | Several complaints contain the keywords *legal notice / authority / refund / cancellation* which must force P1 regardless of classifier confidence |

---

## 5. Rules for using this pack

1. Substitute your own measured numbers before any figure is used in a business case. These are
   assumptions chosen to demonstrate method.
2. Never copy a registration number, rate, duty or deadline from this pack into a real document.
3. Keep the "SAMPLE DATA — ILLUSTRATIVE" label on anything built from it that is shown to anyone.
4. Phone numbers are non-routable placeholders and email addresses use `example.com`.
5. If you replace a dataset with anonymised client material, keep the names fictional and do not
   present any figure from it as the client's actual performance.
