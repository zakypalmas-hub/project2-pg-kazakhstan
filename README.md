# Project 2 — Procter & Gamble Kazakhstan
## Import-to-shelf logistics: warehouse receiving and local labelling as a candidate bottleneck

**Author:** Almas Zhakyp  
**Project type:** Individual Data Analytics capstone  
**Dataset:** 1,600 clean simulated records; 1,624 raw rows including 24 exact duplicate rows  
**Period represented:** 1 January–31 October 2026 (simulated)

## Executive summary
This project investigates whether warehouse receiving and local product-labelling time could be a constraint between imported goods and retail shelf availability in Kazakhstan. Public sources establish that P&G operates at substantial retail scale, uses shelf-photo analytics, has shifted some transport to rail, and is working with Kazakhstan's Ministry of Trade on direct-import logistics, warehouse infrastructure and labelling. However, public sources do **not** provide actual shipment-level warehouse times or stockout rates.

Therefore, `data_raw.csv` and `data_clean.csv` are **simulated**, not company records. They are designed to demonstrate a reproducible analysis. The numeric parameters are explicit analyst assumptions, not reported P&G KPIs. The public facts below are contextual anchors only:
- P&G leadership told Forbes Kazakhstan that products reach more than 24,000 stores and described its long-term distributor partnership.
- P&G's Kazakhstan leadership told Tengrinews that shelf photos are analysed with visual-merchandising algorithms and that shifting part of transport to rail reduced annual CO2 emissions by 1,500 tonnes.
- On 20 November 2026, Kazakhstan's Ministry of Trade and Integration reported a memorandum with P&G Kazakhstan Distribution to develop proposals on direct-import logistics, warehouse infrastructure and local labelling of imported goods.
- A P&G Careers page describes Almaty as a regional office serving six Central Asian markets.

## Business problem, bottleneck and process
**Problem:** Maintaining product availability across a large retail footprint is operationally complex when products are imported and must pass through logistics, warehousing, labelling and distribution. This project does not claim that P&G has a verified stockout problem; it tests a plausible operational risk using simulated data.

**Candidate bottleneck:** Warehouse receiving plus local product-labelling before dispatch. This is a specific process step, not “logistics” in general.

**Process map:** Supplier / origin market → international transport → customs clearance → **warehouse receiving + local label application (candidate bottleneck)** → distributor dispatch → retail store → shelf audit / availability.

**Other possible causes:** (1) customs/documentation delay; (2) warehouse labour, dock or label-application capacity; (3) outbound vehicle/route scheduling; (4) store execution and shelf-replenishment practices. The dataset focuses on reason 2.

## Hypotheses and tests (alpha = 0.05)
1. **H0_1:** Mean warehouse + labelling processing time is equal for high-volume shipments (above the sample median) and lower-volume shipments. **H1_1:** The means differ. Test: two-sided Welch t-test.
2. **H0_2:** Stockout incidence is equal for records with processing time >24 hours and records with processing time <=24 hours. **H1_2:** The proportions differ. Test: Pearson chi-square test of independence.

## Results on the simulated dataset
- High-volume mean processing time: **34.53 h** (n=799); lower-volume mean: **23.82 h** (n=801); Welch t = **22.12**, p = **8.98e-95**. H0_1 is rejected in the simulated data.
- Stockout share for processing <=24 h: **11.0%** (n=537); for >24 h: **26.4%** (n=1063); difference = **15.4 percentage points**, risk ratio = **2.41**; chi-square = **49.96**, df = 1, p = **1.57e-12**. H0_2 is rejected in the simulated data.

**Interpretation warning:** These significant results are consequences of the transparent simulation design. They are not evidence that P&G's real stockout risk is 2.41 times higher. No real P&G operational dataset was available.

## Recommendation
Do not invest based on this simulation. First request a privacy-safe, anonymized operational extract with shipment ID, SKU/category, origin, arrival date, customs release, warehouse receipt, label application start/end, dispatch, store receipt, store audit, units, region and transport mode. Measure queue time, processing time, utilization, backlog and stockout outcome. If the real data confirms a constraint:
1. **Exploit:** pre-validate label artwork/data and pre-stage label files; reduce idle time at receiving.
2. **Subordinate:** schedule inbound arrivals and dispatch waves to measured labelling capacity.
3. **Elevate:** add shifts, equipment or warehouse capacity only if utilization/backlog remains high.
4. **Repeat:** re-measure after the bottleneck moves.

## Data files and reproducibility
- `data_raw.csv` — raw simulated file with mixed date/case/decimal formats, missing values, five impossible distance entries and 24 exact duplicate rows.
- `data_clean.csv` — 1,600-row normalized dataset.
- `generate_data.py` — deterministic synthetic-data generator (seed 42).
- `analysis.ipynb` — cleaning steps, validation checks, two hypothesis tests and interpretation.
- `data_note.pdf` — source note, limitations and complete data dictionary.
- `memo.pdf` — one-page management memo.

To regenerate the raw sample, run `python generate_data.py` from the repository root. Then run the notebook with Python 3, pandas, numpy and scipy installed. Random seed used to generate the dataset: **42**.

## Public sources (accessed 25 November 2026)
1. Forbes Kazakhstan, interview with P&G Europe leadership, retail footprint and distributor partnership: https://forbes.kz/articles/loik-tassel-procter-gamble-kazahstan-zanimaet-osoboe-mesto-vnashem-biznese/
2. Tengrinews, P&G Kazakhstan anniversary interview; shelf-photo algorithms and rail transport / CO2 statement: https://tengrinews.kz/kazakhstan_news/30-let-pg-v-kazahstane-put-k-liderstvu-i-doveriyu-586252/
3. Ministry of Trade and Integration of Kazakhstan, 20 Nov 2026 memorandum with P&G Kazakhstan Distribution on direct-import logistics, warehouse infrastructure and labelling: https://www.gov.kz/org/mti/press/news/details/1333795?lang=ru
4. P&G Careers, Kazakhstan location and regional office: https://www.pgcareers.com/global/en/locations/kazakhstan
5. Market Inside Data, P&G Kazakhstan Distribution trade-intelligence overview; reports 30.36K import shipments and $56.12M import turnover for Sep 2025–Aug 2026 (commercial secondary source; contextual scale anchor only): https://www.marketinsidedata.com/en/company/procter-gamble-kazakhstan/588841ddfc67ee69c6b8e229bcb4b5db

**Bias and limitations:** The dataset represents a synthetic sample of shipment/store replenishment records from Jan–Oct 2026, not a census of P&G activity. It excludes actual supplier contracts, SKU-level demand, real store audits, seasonal promotions, returns, inventory policies and actual warehouse capacity. Assumptions may overstate or understate delays and stockout relationships. Results must not be used to rank employees, suppliers, stores or regions, nor presented as actual company performance. The simulation includes only five regions and five broad product categories and may not reflect the full distribution network.
