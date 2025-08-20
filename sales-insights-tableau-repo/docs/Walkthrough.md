# Walkthrough

## AIMS Grid
**Purpose** — Unlock sales insights for the sales team, automate updates, and cut manual data-gathering time.

**Stakeholders** — Sales Director, Marketing, Customer Service, Data & Analytics, and IT.

**End Result** — An automated Tableau dashboard delivering quick, current sales insights for data-driven decisions.

**Success Criteria**
- Dashboards surface the latest sales order insights.
- Sales can demonstrate ~10% cost savings through better decisions.
- Analysts save ~20% of their time by eliminating manual data gathering.

## Architecture Overview
- **OLTP (MySQL)** — mission‑critical, write‑optimized system for day‑to‑day transactions.
- **ETL** — copy/transform data into an analysis‑friendly shape.
- **Analytics** — Tableau consumes the modeled data (star‑style: Transactions as **fact**; supporting **dimensions**).

## Project Steps
1. **Run basic SQL queries in MySQL** to validate data.
2. **Connect Tableau Desktop to MySQL** and bring in the required tables.
3. **Data Modeling** — build a star schema with **Transactions** at the center.
4. **ETL inside Tableau** (see `ETL.md`) to clean data and create calculated fields.
5. Build sheets: KPIs, trends by month/year, top customers/products, and market/city views.
6. Assemble the **dashboard** with interactive filters (region, city, product, customer).
7. Export a **PDF** of the final dashboard and (optional) **PNGs** per sheet.

## Tableau Tips
- `Ctrl + Shift + B` — increase font size.
- `Ctrl + B` — decrease font size.
