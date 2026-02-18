# COVID Analytics Model Demo Script
## Power BI Modeling MCP - Customer Demonstration

---

##  Demo Objective
Show how the Power BI Modeling MCP enables infrastructure-as-code approach to semantic model development with a complete COVID analytics use case.

**Duration:** 15-20 minutes  
**Audience:** Data engineers, BI developers, IT architects

---

##  PHASE 1: Introduction & Setup (2 minutes)

### [Opening Statement]
"Today I'm going to show you how we built a complete COVID analytics model using the Power BI Modeling MCP - a Model Context Protocol server that lets you treat your Power BI semantic models as code. Instead of clicking through the UI, we'll use commands to create measures, relationships, security roles, and more - all version-controlled and repeatable."

### [Power BI Desktop]
1. Show Power BI Desktop - COVID Testing.pbix file open
2. Point to Data pane - "We have 79 measures organized in 17 folders"
3. Show Report pages - Flip through: Executive Dashboard, Regional Analysis, Time Intelligence, Advanced Analytics

### [MCP Window]
4. Show the chat/command window
5. Run command to get model statistics

**Key Talking Point:** "This is where we control the model programmatically"

---

##  PHASE 2: Model Statistics & Health Check (3 minutes)

### [Command to Run]
``
Get model statistics for the connected database
``

### [Expected Output]
- **78 measures** across multiple tables
- **10 tables** (including calculation groups)
- **61 columns** (including calculated columns)
- **3 relationships** (COVID  StateDim, COVID  Calendar)
- **3 security roles** (Regional Manager, State Manager, Executive)
- **3 cultures** (en-US, es-ES, fr-FR)

### [Narration]
"In one command, we see the entire model structure: 78 measures, 10 tables, 3 relationships, 3 security roles, 3 cultures for multi-language support. This is our starting point."

---

##  PHASE 3: Executive Dashboard Walkthrough (3 minutes)

### [Power BI Desktop]
1. Click "Executive Dashboard" page
2. Point to KPI cards at top:
   - 53 billion total cases
   - 698 million deaths
   - 1.32% case fatality rate

3. Point to state table with per-capita metrics
4. Point to geographic map visualization

### [Key Insight]
"These per-capita calculations require population data. Let me show you how we added that programmatically."

### [Command to Run]
``
Get the Population column from StateDim table
``

### [Expected Output]
Calculated column with 2020 Census data for all 50 US states using SWITCH statement

### [Narration]
"This calculated column contains 2020 Census data for all 50 states. We created this with a single command - no manual data entry. This is the foundation for all per-capita calculations like 'Cases Per 100K Population'."

---

##  PHASE 4: Regional Analysis - Drill-Through Demo (2 minutes)

### [Power BI Desktop]
1. Click "Regional Analysis" page
2. Point to the clustered bar chart showing 4 regions
3. Click the drill-down arrow (top-right of visual)
4. Click on "South" region bar  Visual drills to show Southern states
5. Click the "up" arrow to return to regions

### [Narration]
"Notice the hierarchy - Region to State. This gives business users interactive exploration without writing code. The regional groupings were also added programmatically."

### [Command to Run]
``
Get the Region column from StateDim table
``

### [Expected Output]
Calculated column with SWITCH statement mapping states to Census regions (Northeast, South, Midwest, West)

### [Narration]
"This SWITCH statement maps states to Census regions. Again, created with code - reproducible, testable, version-controlled."

---

##  PHASE 5: Time Intelligence - The Showstopper (4 minutes)

### [Power BI Desktop]
1. Click "Time Intelligence" page
2. Point to the Time Intelligence slicer
3. Point to the matrix showing Year/Month with Total Cases

### [Narration]
"Now watch this. This is a calculation group - one of the most powerful features we can create through the MCP."

### [Interactive Demo - Click Through These Options]
1. **Select "Current"**  Matrix shows actual case numbers
2. **Select "YTD"**  Matrix changes to year-to-date cumulative
3. **Select "Prior Year"**  Matrix shows last year's values
4. **Select "YoY Growth %"**  Matrix shows growth percentages

### [Narration During Clicking]
"Same measure, different time perspectives - no duplicate measures needed. This is the calculation group doing dynamic time intelligence."

### [Command to Run]
``
List calculation items in the Time Intelligence calculation group
``

### [Expected Output]
6 calculation items: Current, YTD, PY, vs PY %, MTD, QTD

### [Narration]
"Six calculation items in this group. Each one transforms ANY measure you add to a visual. This eliminates measure proliferation - instead of creating [Total Cases], [Total Cases YTD], [Total Cases PY], [Total Cases YoY %]... you create ONE measure and the calculation group does the rest."

**This is the WOW moment of the demo - emphasize the power!**

---

##  PHASE 6: Row-Level Security Demo (3 minutes)

### [Narration]
"Let's talk about security. This model has three security roles configured through the MCP."

### [Command to Run]
``
List security roles in the model
``

### [Expected Output]
- **Regional Manager** - Filters to specific US regions
- **State Manager** - Filters to specific US states
- **Executive** - Full access, no filters

### [Power BI Desktop - RLS Testing]
1. Go to **Modeling tab**
2. Click **"View as Roles"**
3. Select **"Regional Manager"**
4. In the text box, type: **"West"**
5. Click **OK**

### [Observe Changes]
- All visuals now show only West region data
- Executive Dashboard numbers changed
- Regional Analysis shows only West region
- State table filtered to Western states only

### [Power BI Desktop]
6. **Modeling tab  Stop Viewing** (return to full data)

### [Narration]
"RLS enforced at the semantic layer - applies everywhere this dataset is consumed: reports, Excel, Power BI mobile, embedded scenarios. All created through code."

---

##  PHASE 7: Advanced Analytics Deep Dive (2 minutes)

### [Power BI Desktop]
1. Click "Advanced Analytics" page
2. Point to each card and explain:

| Card | Value | Explanation |
|------|-------|-------------|
| **Peak Date** | 1/10/2021 | "Identifies January 10, 2021 as the pandemic peak during Alpha variant surge" |
| **Days Since Peak** | 355 | "355 days since maximum impact - about 1 year" |
| **Growth Rate %** | -22.8% | "Week-over-week decline of 22.8%" |
| **Trend Classification** | Declining | "Automatically classifies as Declining based on growth rate" |
| **Doubling Time** | -- | "Blank because growth is negative - can't double when declining" |

### [Command to Run]
``
Get the Trend Classification measure
``

### [Expected Output]
DAX expression showing:
- CurrentWeek vs PriorWeek calculation
- SWITCH logic: >10% = Rising, <-10% = Declining, else Stable

### [Narration]
"Here's the DAX. Notice it's self-documenting - variables with clear names, business logic explicit. This is what infrastructure-as-code gives you: auditability and maintainability."

---

##  PHASE 8: Multi-Language Support (1 minute)

### [Command to Run]
``
List cultures in the model
``

### [Expected Output]
- **en-US** (English) - Base language
- **es-ES** (Spanish) - 11 translations
- **fr-FR** (French) - 5 translations

### [Narration]
"This model supports English, Spanish, and French. The same model serves global audiences - measure names, descriptions, and column headers translated. All managed through the MCP."

---

##  PHASE 9: Infrastructure-as-Code Value Proposition (2 minutes)

### [Summary Slide - Recap What We Built]

**Model Statistics:**
-  78 measures organized in 17 display folders
-  3 relationships connecting fact to dimensions
-  3 security roles for multi-tenant scenarios
-  3 cultures with translations
-  2 calculation groups (Time Intelligence, Currency Conversion)
-  COVID Waves reference table with 7 pandemic phases
-  Per-capita calculations using Census population data

### [Value Proposition - Why This Matters]

**1. Version Control**  
Every measure, relationship, role is text. Check it into Git, review changes in PRs, rollback if needed.

**2. Repeatability**  
This entire model can be recreated from scripts. No "tribal knowledge" in someone's head.

**3. Testing**  
We can write automated tests for DAX measures, validate relationships, check security rules.

**4. CI/CD**  
Deploy models through pipelines. Dev  Test  Prod with approval gates.

**5. Documentation**  
The code IS the documentation. Measure expressions, descriptions, folder structure - all queryable.

**6. Scale**  
Creating 78 measures manually? Days. Through MCP? Minutes. Batch operations on 50 measures at once.

**7. Governance**  
Audit who changed what, when. Enforce naming standards. Require peer review before production.

---

##  PHASE 10: Q&A and Next Steps (2 minutes)

### Common Questions & Answers

**Q: "Does this work with Power BI Service?"**  
A: "Yes - through XMLA endpoints. Premium/Fabric workspaces support read-write XMLA. Same commands work against cloud-hosted models."

**Q: "What about existing models?"**  
A: "Connect to any model - Desktop, Service, Analysis Services. Read existing measures, modify them, or build net-new. It's not just for greenfield projects."

**Q: "Learning curve?"**  
A: "If you know DAX and understand semantic models, you're 90% there. The MCP commands are intuitive - create, update, delete, get, list."

**Q: "Integration with other tools?"**  
A: "Yes - it's a Model Context Protocol server. Works with Claude Desktop, VS Code, or any MCP client. Today we used GitHub Copilot."

**Q: "Cost?"**  
A: "The MCP server is open-source. You need Power BI Premium or Fabric for XMLA write access in production. Desktop is free for development."

**Q: "Performance impact?"**  
A: "None on query performance - we're only changing metadata. The model performs identically whether created through UI or MCP."

**Q: "Can I export my existing model to scripts?"**  
A: "Yes - use the export commands to generate scripts from any existing model. Great for migration or documentation."

---

##  Handoff Materials

### What to Send After Demo

1. **GitHub Repository Link**  
   [powerbi-modeling-mcp](https://github.com/microsoft/powerbi-modeling-mcp)

2. **Demo Model File**  
   COVID Testing.pbix (with all 78 measures, relationships, roles)

3. **Sample Scripts**  
   - Create measures script
   - Add calculated columns script
   - Configure RLS script
   - Setup calculation groups script

4. **Documentation Package**  
   - All 78 measures with business definitions
   - DAX pattern library
   - Naming conventions guide
   - Deployment pipeline templates

### Getting Started Steps

**Week 1: Explore**
- Clone the repo
- Connect MCP to Power BI Desktop
- Run read-only commands (Get, List)
- Explore existing models

**Week 2: Build**
- Create test measures
- Add calculated columns
- Setup basic relationships
- Practice batch operations

**Week 3: Productionize**
- Connect to Premium workspace (XMLA)
- Setup version control
- Create deployment pipeline
- Write automated tests

**Week 4: Scale**
- Migrate existing models to scripts
- Setup peer review process
- Document standards
- Train team

---

##  Demo Closing

### [Final Statement]
"We've shown you how the Power BI Modeling MCP transforms semantic model development from manual, error-prone UI work into code-first, automated, governed processes. You get version control, testing, CI/CD, and documentation for free - all while building faster and with higher quality."

"The COVID model we built today? 78 measures, 3 security roles, multi-language support, calculation groups - all created through commands. This is the future of enterprise BI development."

**Questions? Let's schedule a follow-up to discuss your specific use cases.**

---

##  Demo Checklist

### Pre-Demo Setup
- [ ] Power BI Desktop open with COVID Testing.pbix
- [ ] MCP server connected (verify connection in chat)
- [ ] Report pages created: Executive Dashboard, Regional Analysis, Time Intelligence, Advanced Analytics
- [ ] Year slicer ready on Advanced Analytics page
- [ ] Screenshot of Data pane showing measure folders (backup)

### During Demo
- [ ] Run model statistics command (Phase 2)
- [ ] Show Population column query (Phase 3)
- [ ] Show Region column query (Phase 4)
- [ ] Demonstrate Time Intelligence slicer clicks (Phase 5)
- [ ] Show RLS "View as Role" (Phase 6)
- [ ] Query Trend Classification measure (Phase 7)
- [ ] List cultures command (Phase 8)

### Post-Demo
- [ ] Send GitHub repo link
- [ ] Share demo .pbix file
- [ ] Email sample scripts
- [ ] Schedule follow-up meeting

---

##  Key Success Metrics

**What makes this demo successful:**
- Audience sees code-first approach in action
- Clear understanding of IaC benefits for BI
- At least 2 "wow" moments (Time Intelligence, RLS)
- Questions about specific use cases (engagement)
- Request for follow-up or POC (intent to adopt)

**Total Duration:** 18-20 minutes  
**Key Takeaway:** Infrastructure-as-code for Power BI semantic models enables governance, repeatability, and scale that manual development cannot match.

---

*Document prepared: December 15, 2025*  
*Demo Model: COVID Testing Analytics*  
*Technology: Power BI Modeling MCP v0.1.9*
