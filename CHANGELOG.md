# Release History

## COVID-19 Analytics Demo

Built on top of the Power BI Modeling MCP Server to demonstrate enterprise-grade semantic model development through AI-assisted natural language prompts.

- 79 DAX measures across 20 categories (executive KPIs, epidemiological metrics, data quality, forecasting, and more)
- 10 tables with 61 columns, including calculated columns embedding Census population data
- 3 row-level security roles (Regional Manager, State Manager, Executive)
- 2 calculation groups (Time Intelligence with 6 items, Currency Conversion)
- 3 cultures with Spanish and French translations
- Per-capita analysis using 2020 US Census data via DAX SWITCH statements
- COVID Waves reference table for pandemic phase classification

---

*The entries below document the upstream [Power BI Modeling MCP Server](https://github.com/microsoft/powerbi-modeling-mcp) releases that this project is built on.*

## 0.1.9

- Bug fixes: connection name issue, null-ref dropping tables and others
- Support for ARM based processors

## 0.1.8

- Documentation updates

## 0.1.7

- Bug fixes
- Automatic connection names
- Execution metrics option when running DAX queries
- Tool description updates
  
## 0.1.5

- Bug fixes
- Optimization to trace operations
- DAX query execution metrics
- User confirmation before running DAX queries

## 0.1.1

- Bug fixes
- MCP prompts to facilitate connections: /ConnectToPowerBIDesktop; /ConnectToFabric; /ConnectToPowerBIProject
- Support for Direct Lake entity partitions
- Trace operations


## 0.1.0

- Initial release 


