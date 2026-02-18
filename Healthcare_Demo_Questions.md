# Healthcare Customer Demo Questions
## Power BI Modeling MCP - Advanced Demo Scenarios

---

##  HEALTHCARE-SPECIFIC DEMO QUESTIONS

###  CATEGORY 1: Data Quality & Governance
*Healthcare customers obsess over data quality - show them you can audit it programmatically*

#### Q1: Show me all measures that might have calculation errors or return BLANK values
**MCP Command:**
`
List all measures and check their state - identify any with SemanticError
`
**Why it matters:** Healthcare can't afford wrong calculations in clinical dashboards

---

#### Q2: Which tables have the most recent data refresh? When was the last update?
**MCP Command:**
`
Get partition details showing last refresh timestamps
`
**Why it matters:** Regulatory compliance requires knowing data freshness

---

#### Q3: Show me all columns that don't have proper descriptions - documentation gaps
**MCP Command:**
`
List all columns and filter where Description is empty
`
**Why it matters:** HIPAA audits require documented data lineage

---

###  CATEGORY 2: Clinical Insights (Impressive Queries)

#### Q4: What were the top 5 deadliest weeks during the pandemic?
**MCP Command:**
`
Execute DAX query:
EVALUATE
TOPN(
    5,
    ADDCOLUMNS(
        SUMMARIZE(Calendar, Calendar[Year], Calendar[Month]),
        "Total Deaths", SUM(COVID[Daily deaths]),
        "Total Cases", SUM(COVID[Daily cases]),
        "CFR", DIVIDE(SUM(COVID[Daily deaths]), SUM(COVID[Daily cases]))
    ),
    [Total Deaths],
    DESC
)
`

---

#### Q5: Which states had case fatality rates above the national average?
**MCP Command:**
`
Use [State vs National Average] measure to identify outliers
Query for states where measure > 0
`

---

#### Q6: Show me the correlation between testing rates and case positivity
**MCP Command:**
`
Get [Tests Per Case] and [Positive Test Rate] measures
Execute comparative DAX query
`

---

###  CATEGORY 3: Model Introspection (The "Wow" Factor)

#### Q7: Show me all measures that reference population data
**MCP Command:**
`
List measures from COVID measures table
Filter where Expression contains "Population"
`
**Expected Results:**
- [Cases Per 100K Population]
- [Deaths Per 100K Population]
- [Attack Rate]
- [14-Day Notification Rate]

---

#### Q8: Which measures are marked as SENSITIVE and who can see them?
**MCP Command:**
`
List measures where Description contains "[SENSITIVE"
Get security roles and effective permissions
`
**Found:** [Case Fatality Rate %] - marked "[SENSITIVE: Executive level only]"

---

#### Q9: Show me the DAX expression for your most complex measure
**MCP Command:**
`
Get measure: [Executive Summary]
Display full DAX expression
`
**Why impressive:** Shows AI-generated narrative measure with multiple VARs and business logic

---

###  CATEGORY 4: Security & Compliance

#### Q10: Show me all security roles and what data each role can access
**MCP Command:**
`
List all security roles
Get effective permissions for each role
`
**Expected Output:**
- Regional Manager  Filters StateDim by Region
- State Manager  Filters StateDim by State
- Executive  Full access, no filters

---

#### Q11: Get the filter expressions for Regional Manager role - what's the DAX rule?
**MCP Command:**
`
Get table permission for role "Regional Manager" on table "StateDim"
Show FilterExpression property
`

---

#### Q12: Are there any columns with PHI/PII data that lack proper security?
**MCP Command:**
`
List all columns
Filter for columns containing: "Name", "Address", "SSN", "DOB", "MRN"
Cross-reference with tables that have RLS applied
`
**Security Check:** Ensure sensitive columns are protected

---

###  CATEGORY 5: Performance & Optimization

#### Q13: Which measures have the longest execution times?
**MCP Command:**
`
Execute multiple complex measures with getExecutionMetrics=true
Compare executionTimeMs values
Identify performance bottlenecks
`

---

#### Q14: Show me relationships in this model - any missing foreign keys?
**MCP Command:**
`
List all relationships
Show cardinality (Many-to-One expected for fact-to-dim)
Identify any Many-to-Many relationships
`
**Expected Output:**
- COVID  StateDim (Many-to-One via StateFIPS)
- COVID  Calendar (Many-to-One via Date, Bidirectional)

---

#### Q15: Explain why the Calendar relationship is bidirectional - is that a risk?
**Discussion Point:** Calculation groups require bidirectional filtering to work properly
**MCP Command:**
`
Get relationship: COVID_Date_Calendar_Date
Show crossFilteringBehavior property
`

---

###  CATEGORY 6: Multi-Language & Localization

#### Q16: What languages does this model support? Show me Spanish translations
**MCP Command:**
`
List cultures
Show translation counts per culture
`
**Expected Output:**
- en-US: 0 translations (base language)
- es-ES: 11 translations
- fr-FR: 5 translations

---

#### Q17: Show me all Spanish translations - what terms did we localize?
**MCP Command:**
`
List object translations for culture "es-ES"
Show ObjectType, Property, and Value for each
`

---

###  CATEGORY 7: Advanced - Calculation Groups

#### Q18: Explain how Time Intelligence works without creating 50 measures
**MCP Command:**
`
Get calculation group: Time Intelligence
List calculation items
Get item: YTD to show expression
`
**Key Point:** SELECTEDMEASURE() function applies transformation to ANY measure

---

#### Q19: Show me all calculation items in Time Intelligence group
**MCP Command:**
`
List items in calculation group "Time Intelligence"
`
**Expected Output:**
- Current
- YTD
- PY (Prior Year)
- vs PY % (Year-over-Year Growth)
- MTD
- QTD

---

#### Q20: Can you create a new calculation item on the fly?
**MCP Command:**
`
Create calculation item "Rolling 12 Months" in Time Intelligence
Expression: CALCULATE(SELECTEDMEASURE(), DATESINPERIOD(Calendar[Date], MAX(Calendar[Date]), -12, MONTH))
`
**Demo Impact:** Live creation shows infrastructure-as-code power

---

##  THE "KILLER DEMO" SEQUENCE

### Start Simple, Build to Impressive:

1. **"How many measures?"** 
   - GetStats  78 measures

2. **"Show me one"** 
   - Get [Executive Summary]  Complex AI narrative with 7 VARs

3. **"Who can see sensitive data?"** 
   - List security roles  3 roles with different access levels

4. **"Test it live"** 
   - Execute DAX query  Real-time data retrieval

5. **"What if Spanish users need this?"** 
   - Show cultures & translations  11 Spanish translations configured

6. **"How do you avoid creating 100 time-based measures?"** 
   - Explain calculation groups  1 measure  6 time perspectives = 6 variations

7. **"Prove data quality is monitored"** 
   - Show measures: [Data Completeness %], [Anomaly Days Count], [Zero Reporting Days]

---

##  THE CLOSING QUESTION (Always Gets Them)

**"How long would it take your team to manually recreate these 78 measures, test them, document them, add translations, configure security, and ensure consistency?"**

**Manual Answer:** Weeks, maybe months.

**With MCP:** Hours. And it's all in Git.

---

##  HEALTHCARE-SPECIFIC VALUE PROPS TO EMPHASIZE

 **Regulatory Compliance**
- "Every measure change is tracked in version control - audit-ready"
- Show Git history of DAX changes with approval workflows

 **Data Lineage**
- "We can show exactly how [Case Fatality Rate] is calculated, what data it uses, who can see it"
- Display full dependency graph programmatically

 **Multi-Tenant Security**
- "Same model serves regional managers, state managers, executives - RLS enforced everywhere"
- Demo role testing with View As Roles

 **Clinical Accuracy**
- "Automated testing catches DAX errors before they reach clinical dashboards"
- Show validation scripts that check for SemanticError state

 **Speed to Insights**
- "New epidemiological metrics deployed in minutes, not weeks"
- Live create a new measure and immediately use it

 **Enterprise Scale**
- "Same approach works for 10 measures or 10,000"
- Show batch operations creating 10 measures in one command

---

##  TECHNICAL QUESTIONS (For IT Architects)

### Q21: How do you handle environment promotion (Dev  Test  Prod)?
**Answer:**
- Export model to TMDL format
- Store in Git repo with branching strategy
- CI/CD pipeline validates DAX syntax
- Automated deployment via XMLA endpoints

### Q22: Can this integrate with existing DevOps pipelines?
**Answer:**
- Yes - MCP is command-line friendly
- Azure DevOps tasks can call MCP operations
- GitHub Actions can trigger model deployments
- PowerShell/Python wrappers available

### Q23: What about disaster recovery?
**Answer:**
- Export entire model to TMDL  backup as code
- Can reconstruct model from Git history
- No "lost work" if someone overwrites a measure

### Q24: Performance at scale?
**Answer:**
- Batch operations for bulk changes (create 100 measures in one transaction)
- Connection pooling for concurrent operations
- Incremental refresh compatible

### Q25: How do you test DAX measures?
**Answer:**
- Execute measure with known test data
- Compare result to expected value
- Automated regression testing possible
- Example: Test [Case Fatality Rate] = 1.32% for full dataset

---

##  SAMPLE DEMO SCRIPT

### Opening (30 seconds)
"Let me show you how we manage 78 measures, 3 security roles, and 3 languages across this COVID analytics model - all through code."

### Model Overview (1 minute)
[Run GetStats command]
"10 tables, 78 measures, 3 relationships. Let's drill into what makes this interesting."

### Data Quality (2 minutes)
[Show Data Completeness %, Anomaly Days Count]
"Built-in monitoring. Auditors love this."

### Security Demo (3 minutes)
[List roles, test Regional Manager role]
"Watch how the entire dashboard filters to West region only."

### Time Intelligence (3 minutes)
[Switch between Current, YTD, YoY Growth %]
"One measure, infinite time perspectives. No measure explosion."

### Live Query (2 minutes)
[Execute DAX query showing top states by CFR]
"Ad-hoc analysis without opening Excel."

### Closing (1 minute)
"All of this is version-controlled, testable, and deployable through pipelines. Questions?"

**Total Time:** 12 minutes

---

##  OBJECTION HANDLERS

### "We already use Tabular Editor"
**Response:** "Great! MCP complements Tabular Editor. Think of it as the API layer - perfect for automation, CI/CD, and programmatic access. Tabular Editor is still excellent for visual model design."

### "Our analysts don't write code"
**Response:** "They don't need to! Analysts use Power BI Desktop. MCP is for IT/DevOps to implement governance, testing, and deployment automation. Analysts benefit from better data quality and faster measure deployment."

### "What about licensing?"
**Response:** "MCP itself is open-source. You need Power BI Premium or Fabric for XMLA write access in production. Desktop development is free. Same licensing you already have."

### "Security concerns?"
**Response:** "MCP respects existing Power BI security. Authentication required. RLS enforced. All operations logged. More secure than manual changes because there's an audit trail."

### "What if Microsoft changes the API?"
**Response:** "MCP uses Microsoft's official TOM (Tabular Object Model) - the same API Power BI Desktop and Tabular Editor use. It's stable and supported."

---

##  RESOURCES TO SHARE

- GitHub Repo: powerbi-modeling-mcp
- Sample Models: COVID Testing.pbix
- Documentation: Full API reference
- Video Tutorials: Getting Started series
- Community: Discord/Slack channel

---

**Last Updated:** December 15, 2025
**Model:** COVID Testing Demo (78 measures, 3 roles, 3 cultures)
**Version:** v1.0
