# COVID-19 Analytics: Building an Enterprise Semantic Model with AI and MCP

I built a complete, production-grade Power BI COVID-19 analytics model entirely through natural language conversations with an AI agent. No manual UI clicks. No drag-and-drop. Every measure, relationship, security role, translation, and calculation group was created programmatically using the [Power BI Modeling MCP Server](https://github.com/microsoft/powerbi-modeling-mcp) - Microsoft's open-source Model Context Protocol server that connects AI agents to Power BI semantic models.

This repository documents what I built, how the model is structured, and why this approach matters for enterprise BI development.

![Architecture](docs/img/e2e-diagram.png)

---

## The Idea

Traditional Power BI semantic model development means clicking through the Desktop UI - creating measures one at a time, manually configuring security roles, hand-typing translations. It works, but it doesn't scale, isn't version-controlled, and can't be automated.

The Power BI Modeling MCP Server changes that. It exposes every modeling operation as a tool that AI agents can call. I wanted to push this to its limit: **could I build an entire enterprise-quality analytics model - dozens of measures, security, localization, calculation groups, data quality monitoring - purely through AI-assisted prompts?**

The answer is yes. Here's what that looks like.

---

## What I Built

### The Model

A COVID-19 analytics semantic model built on US county-level case and death data, enriched with 2020 Census population data and organized into four report pages: Executive Dashboard, Regional Analysis, Time Intelligence, and Advanced Analytics.

| Component | Detail |
|---|---|
| **79 DAX measures** | Across 20 categories - from executive KPIs to epidemiological metrics to data quality checks |
| **10 tables** | Fact table, dimension tables, calculation groups, and a COVID Waves reference table |
| **61 columns** | Including calculated columns that embed Census population data via DAX SWITCH statements |
| **3 relationships** | COVID fact to StateDim (via StateFIPS) and Calendar (bidirectional, required for calculation groups) |
| **3 security roles** | Regional Manager, State Manager, Executive - row-level security enforced at the semantic layer |
| **3 cultures** | English (en-US), Spanish (es-ES, 11 translations), French (fr-FR, 5 translations) |
| **2 calculation groups** | Time Intelligence (6 items: Current, YTD, PY, YoY %, MTD, QTD) and Currency Conversion |

### The Measures

The 79 measures are organized into display folders that reflect how a healthcare analytics team would actually use them:

**Executive KPIs** — Total Cases, Total Deaths, Case Fatality Rate % (marked SENSITIVE for executive-only access), Active Cases

**Core Metrics** — Cases Per 100K, Daily New Cases, Daily New Deaths, Case Fatality Rate

**Trending** — 7-Day Moving Average (smooths daily volatility), Growth Rate % (week-over-week expansion/contraction)

**Time Intelligence** — 7-Day Avg Cases, YTD Cases, Prior Year Cases, % Change vs PY

**Epidemiological** — Estimated R Value (reproduction number from 7-day case growth), Doubling Time Days, 14-Day Notification Rate (WHO standard), Trend Classification (Rising/Stable/Declining based on growth thresholds), Weekly Case Acceleration (surge prediction indicator), Case Acceleration

**Per Capita** — Cases Per 100K Population, Deaths Per 100K Population, Attack Rate (% of population infected)

**Comparative** — Best State, Worst State (by per-capita rate), State vs National Average, Percentile Rank, State Rank Per Capita

**Advanced Analytics** — Running Total Cases, Peak Date, Peak Daily Cases, Days Since Peak, % of Total Cases, Avg Daily New Cases

**Data Quality** — Data Completeness %, Data Freshness (Days), Anomaly Days Count (dates with >3x 7-day average spikes), Zero Reporting Days, County Reporting Rate

**Risk Assessment** — Risk Level (WHO-based: Extreme, High, Moderate, Low, Minimal)

**Narrative & Insights** — Executive Summary (auto-generated narrative with multiple VARs and business logic), Traffic Light Status (Green/Yellow/Red), Is Worst Week, Momentum Score (acceleration + growth combined)

**Forecasting** — 14-Day Forecast (based on current 7-day average and growth rate)

**Testing, Hospitalization, Vaccination, Demographics, Policy** — Placeholder measures scaffolded for future data source integration (Test Positivity Rate, ICU Occupancy %, Vaccine Effectiveness, etc.)

**Metadata** — Updated timestamp, Max date, Methodology, Terms of use, Drill-through button text

### Row-Level Security

Three roles demonstrate multi-tenant data access patterns common in healthcare and government:

- **Regional Manager** - DAX filter on StateDim restricts data to a specific US Census region (Northeast, South, Midwest, West). A Regional Manager for "West" sees only Western states across every report page, every visual, every export.
- **State Manager** - DAX filter on StateDim restricts to individual states. A State Manager for "California" sees only California data.
- **Executive** - No filter expression. Full unrestricted access to all data.

RLS is enforced at the semantic layer - it applies everywhere the dataset is consumed: Power BI reports, Excel connections, Power BI Mobile, embedded scenarios.

### Calculation Groups

The Time Intelligence calculation group is the centerpiece of the model. Instead of creating separate measures for every time perspective (Total Cases, Total Cases YTD, Total Cases PY, Total Cases YoY %...), a single calculation group with 6 items transforms ANY measure dynamically:

- **Current** - Actual values
- **YTD** - Year-to-date cumulative
- **PY** - Prior year (same period)
- **vs PY %** - Year-over-year growth percentage
- **MTD** - Month-to-date
- **QTD** - Quarter-to-date

This eliminates measure proliferation entirely. One measure with 6 calculation items replaces what would otherwise be 6 separate measures - multiply that across dozens of base measures and the savings are enormous.

### Multi-Language Support

Model metadata (table names, column headers, measure names) translated into Spanish (11 translations) and French (5 translations). The same model serves global audiences without duplicating anything. All translations created and managed through MCP commands.

### Per-Capita Analysis

Calculated columns embed 2020 US Census population data directly into the StateDim table using DAX SWITCH statements - population for all 50 states plus territories. This enables per-capita metrics (Cases Per 100K Population, Deaths Per 100K Population, Attack Rate) without requiring a separate population data source.

A similar approach maps states to Census regions (Northeast, South, Midwest, West) via a calculated Region column, enabling the Regional Analysis drill-through page and the Regional Manager security role.

---

## How It Was Built

Every component of this model was created through natural language prompts to an AI agent (GitHub Copilot) connected to the Power BI Modeling MCP Server. The workflow:

1. **Connect** — `Connect to 'COVID Testing' in Power BI Desktop`
2. **Create** — `Create a measure [Total Cases] that sums daily cases` or `Add a calculated column to StateDim with population data for all 50 states`
3. **Batch** — `Create measures for Cases Per 100K Population, Deaths Per 100K Population, and Attack Rate` (batch operations handle multiple objects in one command)
4. **Configure** — `Create a security role 'Regional Manager' that filters StateDim by Region`
5. **Translate** — `Generate Spanish translations for all table and measure names`
6. **Validate** — `Get model statistics` to verify everything, `Execute DAX query` to test measures
7. **Iterate** — `Fix [Running Total Cases] — add missing FILTER condition` or `Create [Weekly Case Acceleration] for surge prediction`

The AI agent calls MCP tools like `measure_operations`, `batch_measure_operations`, `security_role_operations`, `culture_operations`, `calculation_group_operations`, and `dax_query_operations` under the hood. I described what I wanted in plain English; the agent translated that into the right API calls.

---

## Why This Matters

### The infrastructure-as-code argument

| Manual UI Development | MCP-Assisted Development |
|---|---|
| Create 79 measures one at a time in the formula bar | Batch-create measures through natural language - minutes, not days |
| Security roles configured through dialog boxes - no audit trail | Roles created as code - version-controlled, reviewable, reproducible |
| Translations entered manually into the Translations pane | Translations generated programmatically across all objects at once |
| "How was this measure built?" requires finding the person who made it | Every measure expression is queryable, documented, and in Git |
| Changes happen live with no undo beyond Ctrl+Z | All changes version-controlled - review in PRs, rollback when needed |
| Testing means manually checking values in the report | DAX queries can validate measures programmatically |

### What this enables at enterprise scale

- **Version control** - Every measure, role, relationship, and translation is text. Store in Git, review in pull requests, rollback when needed.
- **CI/CD** — Export to TMDL format, deploy through Azure DevOps or GitHub Actions: Dev → Test → Prod with approval gates.
- **Automated testing** - Execute measures with known test data, compare to expected values, catch regressions before production.
- **Governance** - Enforce naming conventions across hundreds of measures. Require peer review. Audit who changed what, when.
- **Documentation as a byproduct** - The model IS the documentation. Measure expressions, descriptions, folder structure - all queryable through MCP.
- **Reproducibility** - This entire model can be recreated from scratch. No tribal knowledge locked in someone's head.

---

## Exploring the Model

### Prerequisites

- [Power BI Desktop](https://powerbi.microsoft.com/desktop/) (free)
- [Visual Studio Code](https://code.visualstudio.com/)
- [GitHub Copilot](https://marketplace.visualstudio.com/items?itemName=GitHub.copilot) + [GitHub Copilot Chat](https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat)
- [Power BI Modeling MCP extension](https://aka.ms/powerbi-modeling-mcp-vscode) for VS Code

### Setup

1. Open the COVID Testing `.pbix` file in Power BI Desktop.
2. Install the [MCP VS Code extension](https://aka.ms/powerbi-modeling-mcp-vscode).
3. In VS Code, open GitHub Copilot Chat and connect:

```
Connect to 'COVID Testing' in Power BI Desktop
```

### Things to try

**Model overview:**
```
Get model statistics for the connected database
```

**Explore a complex measure:**
```
Get the DAX expression for [Executive Summary]
```
The Executive Summary is an auto-generated narrative measure with multiple VARs that assembles key metrics and trend classifications into readable text.

**Inspect security:**
```
List security roles and their filter expressions
```

**Test a DAX query:**
```
Execute DAX query:
EVALUATE
TOPN(5, SUMMARIZE(COVID, StateDim[State], "Cases", [Total Cases]), [Cases], DESC)
```

**Check data quality:**
```
Get the DAX expression for [Data Completeness %]
```

**See calculation group items:**
```
List calculation items in the Time Intelligence calculation group
```

**View translations:**
```
List object translations for culture "es-ES"
```

For full MCP server documentation — available tools, prompts, configuration options — see the [upstream repository](https://github.com/microsoft/powerbi-modeling-mcp).

---

## Repository Structure

```
README.md                  - This document
CHANGELOG.md               - Release history
TROUBLESHOOTING.md         - Diagnosing common MCP server issues
SUPPORT.md                 - How to get help
LICENSE                    - MIT License
CODE_OF_CONDUCT.md         - Microsoft Open Source Code of Conduct
SECURITY.md                - Security reporting policy
docs/img/                  - Architecture diagrams and screenshots
.github/ISSUE_TEMPLATE/    - Bug report and feature request templates
```

---

## Links

- [Power BI Modeling MCP Server](https://github.com/microsoft/powerbi-modeling-mcp) — the upstream open-source project this is built on
- [MCP Server end-to-end demo video](https://aka.ms/power-modeling-mcp-demo)
- [Model Context Protocol specification](https://modelcontextprotocol.io/introduction)
- [Power BI Desktop download](https://powerbi.microsoft.com/desktop/)
- [DAX Reference](https://learn.microsoft.com/dax/)

---

## Legal

[MIT License](LICENSE) | [Code of Conduct](CODE_OF_CONDUCT.md) | [Security](SECURITY.md) | [Support](SUPPORT.md)

The underlying MCP server software may collect usage information. See the [Microsoft Privacy Statement](https://www.microsoft.com/privacy/privacystatement).

This software is provided "as is" without warranties of any kind. Always back up your semantic model before performing operations. LLMs may produce unexpected results — review all changes before committing.

