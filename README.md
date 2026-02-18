# COVID-19 Analytics - Power BI Semantic Model Built with MCP

> A production-grade COVID-19 analytics semantic model created entirely through AI-assisted, infrastructure-as-code workflows using the [Power BI Modeling MCP Server](https://github.com/microsoft/powerbi-modeling-mcp).

![powerbi-modeling-mcp-diagram](docs/img/e2e-diagram.png)

---

## What This Project Is

This repository demonstrates how the **Power BI Modeling MCP (Model Context Protocol) Server** can be used to build a comprehensive, enterprise-quality Power BI semantic model from the ground up - without ever opening the Power BI Desktop modeling UI.

Every measure, calculated column, relationship, security role, translation, and calculation group in this model was authored through natural language prompts to an AI agent connected to the MCP server. The result is a fully functional COVID-19 analytics model that is **version-controlled, repeatable, testable, and deployable through CI/CD pipelines**.

---

## Model at a Glance

| Dimension | Detail |
|---|---|
| **Measures** | 79 DAX measures across 20 categories |
| **Tables** | 10 tables (including calculation groups) |
| **Columns** | 61 columns (including calculated columns with Census data) |
| **Relationships** | 3 (COVID -> StateDim, COVID -> Calendar) |
| **Security Roles** | 3 - Regional Manager, State Manager, Executive |
| **Cultures / Translations** | 3 - English (en-US), Spanish (es-ES), French (fr-FR) |
| **Calculation Groups** | 2 - Time Intelligence (6 items), Currency Conversion |
| **Report Pages** | Executive Dashboard, Regional Analysis, Time Intelligence, Advanced Analytics |

---

## Key Capabilities Demonstrated

### Semantic Modeling as Code
Every object in the model - tables, measures, columns, relationships - was created and modified through MCP commands, not manual UI clicks. This makes the entire model reproducible and auditable.

### 79 DAX Measures Organized in 17 Display Folders
From executive KPIs (`Total Cases`, `Case Fatality Rate %`) to epidemiological metrics (`Estimated R Value`, `Doubling Time Days`) to data quality checks (`Data Completeness %`, `Anomaly Days Count`). Full documentation in [COVID_Measures_Documentation.md](COVID_Measures_Documentation.md).

### Row-Level Security (RLS)
Three security roles provide multi-tenant data access:
- **Regional Manager** - filtered to specific US Census regions
- **State Manager** - filtered to individual states
- **Executive** - unrestricted access

### Calculation Groups for Time Intelligence
A single measure produces six time perspectives (Current, YTD, Prior Year, YoY Growth %, MTD, QTD) - eliminating measure proliferation entirely.

### Multi-Language Support
Model metadata translated into Spanish (11 translations) and French (5 translations), managed programmatically through the MCP server.

### Per-Capita Analysis with Census Data
Calculated columns embed 2020 US Census population data directly into the model, enabling per-capita metrics like `Cases Per 100K Population` and `Deaths Per 100K Population`.

### Advanced Analytics
Epidemiological measures including `Trend Classification`, `Weekly Case Acceleration`, `14-Day Notification Rate` (WHO standard), and an auto-generated `Executive Summary` narrative measure.

---

## Repository Contents

| File | Description |
|---|---|
| [COVID_Measures_Documentation.md](COVID_Measures_Documentation.md) | Complete reference of all 79 DAX measures organized by category with descriptions and business purpose |
| [COVID_Analytics_Demo_Script.md](COVID_Analytics_Demo_Script.md) | Step-by-step 15-20 minute demo script walking through every feature of the model |
| [Healthcare_Demo_Questions.md](Healthcare_Demo_Questions.md) | 25+ advanced demo questions organized by theme: data quality, clinical insights, security, performance, localization |
| [TROUBLESHOOTING.md](TROUBLESHOOTING.md) | Guide for diagnosing and resolving common MCP server issues |
| [CHANGELOG.md](CHANGELOG.md) | Release history for the upstream Power BI Modeling MCP Server |
| [docs/img/](docs/img/) | Architecture diagrams and VS Code screenshots |

---

## Getting Started

### Prerequisites

- [Power BI Desktop](https://powerbi.microsoft.com/desktop/) (free)
- [Visual Studio Code](https://code.visualstudio.com/)
- [GitHub Copilot](https://marketplace.visualstudio.com/items?itemName=GitHub.copilot) + [GitHub Copilot Chat](https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat) extensions
- [Power BI Modeling MCP extension](https://aka.ms/powerbi-modeling-mcp-vscode) for VS Code

### Install the MCP Server

1. Install the [Power BI Modeling MCP VS Code extension](https://aka.ms/powerbi-modeling-mcp-vscode).

   ![vs code install](docs/img/vscode-extension-install.png)

2. Open GitHub Copilot Chat and confirm the **powerbi-modeling-mcp** server is available.

   ![vscode-mcp-tools](docs/img/vscode-mcp-tools.png)

### Connect to the Model

Open the COVID Testing `.pbix` file in Power BI Desktop, then in VS Code:

```
Connect to 'COVID Testing' in Power BI Desktop
```

Verify the connection:

```
Get model statistics for the connected database
```

### Explore the Model

Try these prompts to see the model in action:

```
List all measures and their display folders
```

```
Get the DAX expression for [Executive Summary]
```

```
List security roles in the model
```

```
List cultures and translation counts
```

For a complete guided walkthrough, see the [Demo Script](COVID_Analytics_Demo_Script.md).

---

## Measure Categories

The model's 79 measures span 20 categories:

| Category | Examples |
|---|---|
| **Executive KPIs** | Total Cases, Total Deaths, Case Fatality Rate %, Active Cases |
| **Core Metrics** | Cases Per 100K, Daily New Cases, Daily New Deaths |
| **Trending** | 7-Day Moving Average, Growth Rate % |
| **Time Intelligence** | YTD Cases, PY Cases, % Change vs PY |
| **Advanced Analytics** | Running Total Cases, Peak Date, % of Total Cases |
| **Epidemiological** | Estimated R Value, Doubling Time, Trend Classification, Weekly Case Acceleration |
| **Per Capita** | Cases Per 100K Population, Deaths Per 100K Population, Attack Rate |
| **Comparative** | Best State, Worst State, State vs National Average, Percentile Rank |
| **Data Quality** | Data Completeness %, Anomaly Days Count, Zero Reporting Days |
| **Risk Assessment** | Risk Level (WHO-based classification) |
| **Narrative** | Executive Summary (auto-generated), Traffic Light Status, Momentum Score |
| **Forecasting** | 14-Day Forecast |

See [COVID_Measures_Documentation.md](COVID_Measures_Documentation.md) for the complete reference.

---

## Why Infrastructure-as-Code for Semantic Models?

| Benefit | How This Project Demonstrates It |
|---|---|
| **Version Control** | Every measure and role is text in Git - review in PRs, rollback when needed |
| **Repeatability** | The entire model can be recreated from MCP commands - no tribal knowledge |
| **Testing** | DAX measures can be validated programmatically; security roles can be tested |
| **CI/CD** | Export to TMDL, deploy through pipelines: Dev -> Test -> Prod |
| **Documentation** | The code IS the documentation - queryable, auditable, always current |
| **Scale** | 79 measures created in minutes through batch operations, not days of manual work |
| **Governance** | Audit trail of every change, enforced naming standards, peer review before production |

---

## Demo Resources

This repository includes two comprehensive demo guides:

- **[COVID_Analytics_Demo_Script.md](COVID_Analytics_Demo_Script.md)** - A 15-20 minute structured walkthrough covering model statistics, executive dashboards, regional drill-through, time intelligence calculation groups, row-level security, and advanced analytics. Includes narration scripts and talking points.

- **[Healthcare_Demo_Questions.md](Healthcare_Demo_Questions.md)** - 25+ questions organized into categories (data quality, clinical insights, security, performance, localization, calculation groups) with MCP commands and expected outputs. Includes a "killer demo sequence" and objection handlers.

---

## Built With

This project is built on top of the **[Power BI Modeling MCP Server](https://github.com/microsoft/powerbi-modeling-mcp)** by Microsoft - an open-source MCP server that brings Power BI semantic modeling capabilities to AI agents.

Refer to the [upstream repository](https://github.com/microsoft/powerbi-modeling-mcp) for full documentation on:
- [Available MCP tools and operations](https://github.com/microsoft/powerbi-modeling-mcp#%EF%B8%8F-available-tools)
- [Built-in prompts](https://github.com/microsoft/powerbi-modeling-mcp#%EF%B8%8F-available-prompts)
- [Configuration and settings](https://github.com/microsoft/powerbi-modeling-mcp#%EF%B8%8F-settings)
- [Manual installation for other MCP clients](https://github.com/microsoft/powerbi-modeling-mcp#manual-install)

---

## Troubleshooting

See [TROUBLESHOOTING.md](TROUBLESHOOTING.md) for common issues including MCP server logging, restarting the server, and locating binaries.

---

## Legal

- [MIT License](LICENSE)
- [Microsoft Open Source Code of Conduct](CODE_OF_CONDUCT.md)
- [Security Policy](SECURITY.md)
- [Support](SUPPORT.md)

### Data Collection

The underlying MCP server software may collect usage information and send it to Microsoft. Review the [Microsoft Privacy Statement](https://www.microsoft.com/privacy/privacystatement) for details.

### Disclaimer

This software is provided "as is" without warranties of any kind. Always back up your semantic model before performing operations through any MCP client. LLMs may produce unexpected results - exercise caution and review all changes before committing.
