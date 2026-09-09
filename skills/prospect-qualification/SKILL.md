---
name: prospect-qualification
description: Final manual fit-check for a premium cold-email agency. Given ONE company website (or a CSV of many), visits the site + does external registry/LinkedIn lookups and decides SELECT / SKIP / MANUAL CHECK — the one question is "can this company afford a premium outbound retainer and is there outbound work to do for them?". ICP/industry/geo/size filtering is assumed already done at list-build time. Enforces 10 locked hard disqualifiers (external parent/group/fund that puts the buyer out of reach — a founders' own holding shell does NOT count; free demo/consultation, no B2B motion, staffing shop, low-ticket, micro/consumer customers, government-only, competitor selling outbound/lead-gen, geospatial/GIS/mapping/surveying). Uses parallel MCP / exa MCP for the parent-company check. Triggers on "qualify this company", "is this a good fit", "check this website", "run the fit check", "SELECT or SKIP".
user_invocable: true
---

# Prospect Qualification

## Goal

You are the **final human-style check** on a lead list that has *already* been filtered for ICP, industry, geography, employee size, and keywords. Do **not** re-filter on any of those.

Answer exactly one question per company. **In practice this reduces to three checks (session-3 framing):**

1. **Can we reach the decision-maker?** No real parent / group / fund above them (a founders' own holding shell is fine — see D1), AND a findable decision-maker with a phone / LinkedIn.
2. **Do their case studies / client list show mid-ticket AND high-ticket clients?** Real businesses, not a client base that is *entirely* micro / solopreneur / consumer (D7), and not genuinely commodity work.
3. **Do they trip any hard disqualifier?** Free demo/trial/plan (D3), competitor selling outbound (D9), core staffing/body-shop (D5), government-only client base (D8), no B2B motion (D4).

Clears 1 + 2 and trips none in 3 → **SELECT**.

> **NEVER a reason to SKIP:** the company's own employee count, headcount, revenue size, "they look too small to afford it", funding stage, industry, or geography. Those are filtered at list-build time. Affordability is judged **only** by whether *their* offering is high/mid-ticket (shown by client calibre and the absence of cheap self-serve pricing) — never by how big or how profitable the company itself is.

Output: **SELECT** / **SKIP** / **MANUAL CHECK**, every signal citing where it was found, ending in a one-line verdict.

The full rationale and signal reference is in `methodology.md` next to this file — read it once per session before running.

## Modes

- **Single company** — user gives one URL, wants an answer in chat. Run §Procedure, emit §Output.
- **Batch** — user gives a CSV with a domain/website column. Run §Procedure per row, write `verdict`, `reason`, `flags`, `sources` columns back to a new CSV. Sample 3 rows first, show the user, get a go-ahead, then run the rest. Save to `<input>-qualified.csv`.

## The locked rules

### Hard disqualifiers — ANY ONE = SKIP

| # | Rule |
|---|------|
| **D1** | **A real parent above it puts the buying decision out of reach.** The question D1 actually answers: *is the person who can say "yes" to a retainer reachable, or does the decision sit inside a group / fund above them?* **SKIP** if owned by another **operating company**, a **larger group** (group branding, "part of the X Group" / "a X company", shared group management, several operating companies under one roof), or a **PE / VC / investment fund** holding a controlling or significant stake — you can't sell to the target directly. **NOT D1 — still SELECTable (the exception):** a **pure holding shell owned by the target's own founders / operators as individuals** — a personal / family / tax holdco or an MBO vehicle (e.g. "[Founder] Holding / Ventures / Invest s.r.o.", "X Holding ApS") whose only real asset is this company (± the same founders' other small ventures), no operations of its own, registered purpose "manage own assets / real estate", and **no group branding on the website**. Same people, same reachable decision-maker → treat as independent. **Test: trace the holdco up one level** — reach the target's own founders as individuals → not D1; reach a different company, a fund, or outside people → D1 → SKIP. Confirm "reachable": the same owners are the operating company's directors and are named / on LinkedIn / on the contact page, with no group gatekeeping. |
| **D2** | *(not a disqualifier — the opposite)* Company **owns its own** subsidiaries / smaller companies beneath it → **fine**. Being a parent is fine; being a child is not. |
| **D3** | **Something on the site is offered explicitly FREE.** SKIP if the site offers a **free** demo, **free** trial, **free** consultation / strategy call / audit / workshop, a **free plan / freemium**, "start free", "sign up free", "no credit card required", or self-serve signup. **NOT D3 (fine — keep):** a plain form to fill out for a consultation, a demo, or contact when it is *not* advertised as free — "Book a demo", "Request a demo", "Book a consultation", "Contact us", "Get a quote", "Talk to sales" are all normal B2B sales entry points. **The test:** is the word **free** (zdarma / gratis / bezplatná / "no cost" / "no obligation, free") attached to the offer? Yes → SKIP. Just a contact/demo/consultation form with no "free" → fine. |
| **D4** | **No B2B sales motion.** Sells nothing to other businesses — prop-trading firm, fund trading own capital, pure holding co, internal-only — and the main CTA is "Join us" / careers. |
| **D5** | **Core business is IT staffing / body-leasing / contractor resourcing** billed on day rates. *(A project company that also offers some staff augmentation on the side is NOT auto-skipped — judge the core: read the whole services page and weight by prominence. One "scale your team / dedicated experts / no recruitment hassle" call-out buried under consulting + product + training offerings does NOT trigger D5. It triggers D5 only when team-extension / staff-aug IS the headline pitch.)* |
| **D6** | **Clearly low-ticket / self-serve.** Transparent low public pricing (tens–low-hundreds/mo), instant credit-card checkout, high-volume low-price model. **Judge D6 on THEIR offering — the pricing page, the checkout, the client calibre — NOT on the company's own revenue or headcount.** A small agency with €250k revenue that serves TON, Meopta, Janošík (real mid-market brands) with custom project work is NOT low-ticket. "Too small to afford our retainer" is never a valid SKIP reason (Studio 9 calibration). |
| **D7** | **Customers are purely micro / solopreneur / consumer** (B2C, freelancers, 1–5 person shops: local café, plumber, single salon). **Mid-market customers are fine.** |
| **D8** | **Customer base is EXCLUSIVELY (or all but exclusively) government / public sector** — the company sells only to ministries, state agencies, public hospitals, universities, state-owned enterprises, municipalities, defence/law-enforcement/intelligence, and has essentially no private commercial customers. Reason: a small outbound agency cannot prospect government buyers (tenders, 6–18 mo cycles). **Government being the *majority* of the client base is NOT a skip** — if they also genuinely serve private enterprises (any private companies, corporate/works canteens, private industry, etc.), D8 does NOT fire; you run outbound at the private-sector side. Case studies featuring government bodies are not enough on their own — check whether private companies are also served. **Check the real client base, not the marketing site:** for CZ/SK companies the public-contracts registry (smlouvy.gov.cz, hlidacstatu.cz) lists every state contract by supplier ID — search the IČO. Use it to see how much is government; then check the site/references for private clients before concluding. |
| **D9** | **Competitor — the company itself sells outbound / cold email / lead generation / appointment-setting / SDR-as-a-service.** Cold-email agencies, lead-gen agencies, demand-gen agencies, appointment-setting firms, SDR / BDR outsourcing, sales-development agencies, and BPO / call centres whose service menu includes "outbound campaigns" or "lead generation". They run outbound in-house and will never buy an outbound retainer — and they're a competitor. Check the services / what-we-do page for words like *outbound, lead generation, appointment setting, generování leadů, obchodní schůzky, outboundové kampaně, sales development*. |
| **D10** | **Core offering is geospatial / GIS / mapping / surveying — outbound isn't a viable channel for it.** SKIP if the company's main business is geographic/spatial: aerial or satellite imaging, orthophoto, photogrammetry, LiDAR, geodesy, land surveying, cadastre / land-registry work, cartography, GIS software / platforms / data, mobile mapping, 3D terrain or city models, urban-planning (územní plán) documentation, or spatial analytics as the headline product. Reason: the buyer universe is tiny, heavily municipal / regional / public-infrastructure, and **tender-driven** (6–18 mo RFP cycles), the purchase is infrequent and technical (surveyors + GIS staff + procurement, not one reachable growth buyer), and there is no repeatable "outreach → demo → deal" motion. This holds **even if some clients are private** and even if the company is independent and high-ticket — D10 is about the *service category*, not the client mix (that's D8). A freemium GIS web-portal alongside the surveying business does not rescue it. *(TopGis calibration — flipped SELECT → SKIP. Also flips the earlier Lutra Consulting and ARCDATA PRAHA SELECTs.)* |

### SELECT — must clear ALL of these

1. Independent *for selling purposes* — no operating parent, branded group, or PE/VC/investment fund above it. A holding shell owned by the target's own founders/operators (same reachable decision-maker) is fine (D1).
2. Nothing offered explicitly free — no free demo / trial / plan / consultation / audit (D3). A plain "book a demo" / "book a consultation" / contact form that is *not* called free is fine.
3. Real B2B sales motion — sells a service or product to other businesses (D4).
4. Not low-ticket — custom-quoted, no cheap self-serve; mid-ticket or higher (D6).
5. Customers are mid-market or larger — not purely micro/solopreneur/consumer (D7).
6. Customers are not a government-only client base — some genuine private-enterprise customers exist (D8). Government can be the majority; it just can't be everything.
7. The work has real substance — custom engineering, integration, specialised software, data/analytics, automation, or strategic depth. Exclude only genuinely commodity work (template websites, basic SEO, logo/graphic design).

8. **Not a competitor** — does not itself sell outbound / cold email / lead-gen / appointment-setting / SDR-as-a-service (D9).

9. **Not a geospatial / GIS / mapping / surveying company** (D10) — the service category has a tiny, mostly-public, tender-driven buyer universe that cold outbound can't address.

Clears all 9 and trips none of D1–D10 → **SELECT**.

### Do NOT filter on

Company size / headcount / **revenue size / "too small to afford it"** · funding stage · industry / vertical · geography · "enterprise clients" (mid-market 50–500 employees with real revenue is fine — enterprise is a bonus, not a requirement) · the mere word "Enterprise" on the site (read what it actually refers to — "enterprise ERP we integrate with" ≠ "we sell to enterprises") · a shrinking-revenue year · offshore / low-cost delivery staff. **The list is already size- and geo-filtered — never SKIP on any of these, and never let "small revenue" become an implied D6.**

### "Depends" categories — run through D1–D8 + the 7, do not auto-skip

Custom e-shop / e-commerce build agencies · MSP / IT infrastructure / helpdesk / hardware reselling · generic marketing / SEO / branding / design agencies · mobile-app dev shops · general custom-software dev shops (mixed client base).

## Procedure

### Step 1 — Read the website (every time)

Fetch and read these pages (use `WebFetch`, or exa/parallel `web_fetch` for JS-heavy sites):

1. Homepage — hero, primary CTA
2. `/pricing` (or equivalent) — custom vs cheap self-serve
3. `/customers`, `/references`, `/case-studies`, `/portfolio` — customer size and type
4. `/security`, `/trust`, `/compliance` — procurement infrastructure (SOC 2, ISO 27001, SSO/SAML, SLA, DPA)
5. **Footer** — parent company, copyright entity, "a X company", "part of the Y Group", group branding
6. `/about`, `/company`, `/o-nas` — history, ownership, group structure
7. `/careers`, `/jobs` — sales roles (Enterprise AE, SDR/BDR, Sales Engineer) and seniority
8. **Both language versions** — CEE sites usually have an English + local site and they differ; a key word (e.g. "Enterprise") can be on one only.

If a page reads empty / a claim seems missing, re-fetch the specific sub-page and the other-language version before concluding — the page-to-text reader drops content.

### Step 2 — External parent-company check (D1 — the make-or-break, ALWAYS do this)

The website almost never names its owner. Run a research pass:

1. **Try `parallel` MCP first** (`web_search`, then `web_fetch` on the best hits). If it returns `402 Insufficient credit`, fall back to **`exa` MCP** (`web_search_exa` → `web_fetch_exa`). If both are unavailable, use `WebSearch` / `WebFetch`.
2. Queries to run (fill in the company name + legal suffix):
   - `"<company>" owner OR shareholders OR "parent company" OR acquired OR "part of"`
   - `"<company>" <legal-id / IČO / CVR / company number> shareholders`
   - `site:linkedin.com/company <company>` — headcount + "affiliated" / "part of" pages
3. Best sources, in order:
   - **Local company registry** — the authoritative one:
     - Czech Republic: `justice.cz`, `or.justice.cz`, `rejstrik-firem.kurzy.cz`
     - Slovakia: `orsr.sk`, `finstat.sk`, `finreg.sk`, `overit.sk`
     - Denmark: `cvr.dk`, `proff.dk`, `virk.dk`
     - UK: Companies House (`find-and-update.company-information.service.gov.uk`)
     - Aggregators covering all of the above: `northdata.com`
   - **LinkedIn company page** — headcount (most reliable single number), "part of" links
   - **Crunchbase** — acquisitions, parent org, investors
   - **Product sub-sites / footers** ("X is developed by Y, a member of the Z group") often name the parent when the main About page doesn't
   - Press: `"<company>" acquired OR "becomes part of" OR "backed by"`
4. Read the shareholder / "Jediný akcionár" / "Spoločníci" / "ownership" section:
   - **Individuals hold the shares directly → not D1.** Independent.
   - **A company holds the shares → do NOT stop; trace it one level up and identify what kind of holder it is:**
     - **Pure holding shell owned by this company's own founders / operators as individuals** (personal / family / MBO / tax vehicle — no operations, no staff, registered purpose "manage own assets / real estate", only real asset is this company ± the same founders' other small ventures, and the website shows no group branding) → **NOT D1.** Same reachable decision-maker → independent, can SELECT.
     - **Another operating company, or a real / branded group** ("part of the X Group", "a X company", group logo, group-level management, several operating companies) → **D1 → SKIP.** Decision sits above.
     - **A PE / VC / investment fund** (SICAV, "investiční společnost", named PE/VC) holding a controlling or significant stake → **D1 → SKIP.**
     - Immediate holder is itself a shell → keep tracing up; a non-founder corporate / group / fund owner at **any** level → SKIP.
   - **This company holds shares in *other* smaller companies → D2 → fine**, note it and move on.
   - Registry data can lag a reorg (a group dissolved, a sister company sold) — check the dates.
   - **The founder's / client's firsthand call overrides this check:** if the user says their founder has already spoken to the company and judged it a fit, that beats the registry proxy — SELECT even if a holdco technically sits above. A real conversation with the decision-maker is better evidence than the register.

### Step 3 — Apply the rules

Walk D1–D10, then the 9 SELECT criteria. Decide.

**Exhaust the research before you reach for MANUAL CHECK.** Use exa MCP (parallel MCP when it has credit), LinkedIn, Crunchbase, press, and the local registry — and your own knowledge of the company / its acquirers / its market. Chase the parent-company question up every level. Read the *whole* services page, not one line. MANUAL CHECK is a last resort for a genuine either/or that no public source settles — not a shortcut when a source exists but is inconvenient to find. Most "residuals" can be resolved with one more registry lookup (shareholder list, share pledges, beneficial owner) — do it.

- Genuinely not resolvable from any public source → **MANUAL CHECK**. Do not guess.
  - **MANUAL CHECK output must be tiny.** After the table + verdict, end with a 2–3 sentence plain-English block: (1) the one doubt, stated simply — "I can't tell if X or Y"; (2) why it matters — "X → SKIP, Y → SELECT"; (3) the single question or field that resolves it. No second table, no long checklist. If there are two doubts, one sentence each, lead with the bigger one.
- Client list is anonymised ("a Swiss bank", "an Austrian insurer") → counts toward "sells to mid-market or larger" but flag as *unverified*.

## Output

```
COMPANY: <name> — <URL>

| Signal | Where found | Effect (rule) |
|--------|-------------|---------------|
| <what the site / lookup shows> | <exact URL or external source> | include / exclude / flag (D1–D8 or SELECT #n) |
| ... | ... | ... |

SUMMARY
VERDICT: SELECT | SKIP | MANUAL CHECK
REASON: <1–2 sentences tied to the rules>, each claim with (source: <where>)
IF MANUAL CHECK:
THE DOUBT: <"I can't tell if X or Y" — one sentence>
WHY IT MATTERS: <X → SKIP, Y → SELECT — one sentence>
HOW TO RESOLVE: <the single question to ask on a call, or the one registry field to pull>
```

Rules for the output:
- **Every signal names where it was found** — exact page URL or external source — and which rule it triggers.
- **All sources in English** — English-language source where possible; otherwise the English translation of the exact text + the page URL + "Chrome → right-click → Translate".
- Cover every time: what they sell + ticket level; who their customers are (type / size, private vs government); free demo/trial/consultation (checked across home, about, pricing, references, careers, contact, footer, both languages); parent/holding company above them; whether they own subsidiaries (not a problem); B2B sales motion / main CTA; real substance in the work.
- End with the one-line verdict.

## Batch mode specifics

- Input CSV: any column named `domain`, `website`, `url`, or `company_url` (case-insensitive). Keep all original columns.
- Add columns: `verdict`, `reason`, `flags` (semicolon-separated Dx codes hit or "-"), `sources` (semicolon-separated URLs).
- Sample the first 3 rows, run them, show the user the output table, wait for "go", then run the rest.
- Rate-limit the research pass: ~1 company every few seconds; if the MCP returns 402 / 429, switch provider or pause.
- Save to `<input>-qualified.csv`. Print a summary: N SELECT / N SKIP / N MANUAL CHECK, and the SKIP reason breakdown by Dx.

## Calibration notes (learned from 23 worked examples)

- **D9 competitor is a clean SKIP** — Go Digital! (go-digital.cz) is a BPO whose menu lists "outbound campaigns" + "leads" (appointment-setting) + "email campaigns". Any company selling outbound/lead-gen/appointment-setting/SDR services is out.

- **D1 is about *reachability of the buyer*, not legal structure.** SKIP only when a real parent sits above and owns the decision: an operating company, a branded group, or a PE/VC/investment fund (Synerga → AVANT ENERGY SICAV fund; Controlis → Liftrock group; Senvio → MIBCON Group; Meonzi → "a Trask Company"; Sophia → SUDOP CIT; Shopsys → ABUGO; ANETE → TTC group; Medicalc → Infini One a.s. external majority). **A holding shell owned by the target's own founders / operators is NOT D1** — same reachable decision-maker. UX Fans (→ UXF Ventures s.r.o., the founders' own vehicle) is a SELECT. **This also reverses the earlier NEOXX and Datacon SKIPs** — Aterea s.r.o. (NEOXX's two managers' MBO vehicle) and LHUB Holding ApS (Datacon owner's personal holdco) are founder wrappers, not real parents. Always trace the holdco up one level: founders as individuals → keep; a different company / fund / outside people → skip. It still nearly always needs the external registry lookup. A company owning its *own* subsidiaries is D2 (fine).
- **"Enterprise-only" was relaxed to "mid-market or larger."** Don't skip a company because its clients are mid-market rather than Fortune-500.
- **"Too big" is not a thing. "Too small" is also not a thing.** No headcount, revenue, or funding ceiling OR floor. A 5-person agency on €250k revenue can be a SELECT if it's reachable and serves mid-ticket clients (Studio 9). Never SKIP because the company "looks too small to afford the retainer" — and never turn "small revenue" into an implied D6. D6 is about *their pricing model* (cheap public pricing / self-serve checkout / micro-only clients), not their size.
- **D3 is only about the word "free" (session 3).** SKIP only if something is offered explicitly free — free demo / trial / plan / consultation / audit / workshop, freemium, self-serve signup, "no credit card". A plain "Book a demo" / "Request a demo" / "Book a consultation" / contact form that is NOT called free is **fine** — that's just a normal B2B sales entry point, keep it. ("free audit", "free strategy call", "konzultácia zdarma", "bezplatná konzultácia" still fire.)
- **Prop-trading / own-capital / no-customers businesses are a clean SKIP (D4)** even when rich.
- **Staffing / body-leasing as the *core* business is a clean SKIP (D5);** a project company that also offers staff augmentation is not.
- **D8 fires only on a government-ONLY client base.** Government being the majority is fine as long as private enterprises are also genuinely served (corporate/works canteens, private industry, private company refs) — run outbound at the private side. Don't skip off government-heavy case studies alone; check for private clients first. (ANETE: heavy state-hospital + Ministry of Defence use, but also private works canteens + ŽĎAS + KNL Catering → D8 clears; it skipped on D1 instead.)
- **D10 — geospatial / GIS / mapping / surveying is a clean SKIP regardless of ownership, ticket size, or client mix.** TopGis (topgis.cz — aerial photography, orthophoto, mobile mapping, thermal imaging, GisOnline portal, 3D územní plán, WMS) was flipped SELECT → SKIP: the whole category has a tiny, municipal/regional/public-infrastructure, tender-driven buyer universe and no repeatable outreach→demo→deal motion. Also flips the earlier **Lutra Consulting** (custom QGIS engineering) and **ARCDATA PRAHA** (national Esri/ArcGIS distributor + GIS consulting) SELECTs. Watch for: *geodézie, mapování, ortofoto, letecké/družicové snímkování, fotogrammetrie, LiDAR, kataster, GIS, územní plán, mobilní mapování, 3D model, prostorová analytika*.
