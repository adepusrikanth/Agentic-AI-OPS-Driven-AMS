# Agentic-AI-OPS-Driven-AMS# Agentic AI-OPS Driven AMS — MVP Documentation

## Table of Contents

1. [Project Overview](#1-project-overview)
2. [File Inventory](#2-file-inventory)
3. [Data Dictionary — dummy_incidents.csv](#3-data-dictionary--dummy_incidentscsv)
4. [Data Quality & Profile](#4-data-quality--profile)
5. [Notebook Walkthrough — Agentic_AI_OPS_AMS_MVP.ipynb](#5-notebook-walkthrough--agentic_ai_ops_ams_mvpipynb)
   - [Section 0: Setup & Dependencies](#section-0-setup--dependencies)
   - [Section 1: Data Ingestion](#section-1-data-ingestion)
   - [Section 2: Ticket Clustering](#section-2-ticket-clustering)
   - [Section 3: Cluster Prioritization](#section-3-cluster-prioritization)
   - [Section 4: Knowledge Article (KA) Generation](#section-4-knowledge-article-ka-generation)
   - [Section 5: Enterprise Knowledge Graph (EKG)](#section-5-enterprise-knowledge-graph-ekg)
   - [Section 6: Vector Store & Semantic Search](#section-6-vector-store--semantic-search)
   - [Section 7: Agent & Remediation Enablement](#section-7-agent--remediation-enablement)
   - [Section 8: AI-OPS Brain — Plan Generation & Execution](#section-8-ai-ops-brain--plan-generation--execution)
   - [Section 9: Executive Dashboard](#section-9-executive-dashboard)
   - [Section 10: Architecture Summary](#section-10-architecture-summary)
6. [Technology Stack](#6-technology-stack)
7. [How to Run the Demo](#7-how-to-run-the-demo)
8. [Architecture Diagram (Text)](#8-architecture-diagram-text)
9. [Production Roadmap](#9-production-roadmap)

---

## 1. Project Overview

This project is a **Minimum Viable Product (MVP)** demonstrating an **Agentic AI-OPS-Driven Automated Managed Services (AMS)** solution. It addresses the real-world problem of recurring IT incidents in enterprise systems (SAP ECC, Oracle EBS) by combining machine learning clustering, automated knowledge generation, graph-based reasoning, and orchestrated execution planning.

### Business Problem

Enterprise AMS operations are overwhelmed by repetitive incidents. Human agents spend significant time resolving the same patterns over and over. This MVP demonstrates how AI can:

- **Detect patterns** in incident tickets through clustering
- **Prioritize** which patterns to automate first
- **Generate Knowledge Articles** that codify remediation expertise
- **Build a Knowledge Graph** that maps relationships between systems, issues, causes, and agents
- **Orchestrate resolution** through an AI "Brain" that matches new incidents to known solutions and executes plans

### End-to-End Workflow

```
Incident Tickets (CSV)
        |
        v
  Ticket Clustering (TF-IDF + KMeans)
        |
        v
  Cluster Prioritization (composite scoring)
        |
        v
  Knowledge Article Generation (per cluster)
        |
        v
  Enterprise Knowledge Graph (NetworkX)
        |
        v
  Vector Store for Semantic Search (TF-IDF cosine similarity)
        |
        v
  Agent Registry & Remediation Workflows
        |
        v
  AI-OPS Brain (orchestrator — plan generation & execution)
        |
        v
  Executive Dashboard (KPI visualization)
```

---

## 2. File Inventory

| File | Type | Purpose |
|------|------|---------|
| `dummy_incidents.csv` | CSV (comma-delimited) | 50 synthetic SAP/Oracle incident tickets used as input data |
| `Agentic_AI_OPS_AMS_MVP.ipynb` | Jupyter Notebook | Complete MVP implementation — 37 cells covering all 10 workflow stages |
| `PROJECT_DOCUMENTATION.md` | Markdown | This documentation file |

Both files must be in the **same directory** for the notebook to find the CSV at runtime.

---

## 3. Data Dictionary — dummy_incidents.csv

The CSV contains **50 rows** (one per incident) and **16 columns**. Every row represents a resolved production incident from either SAP ECC or Oracle EBS.

| # | Column | Data Type | Example Value | Description |
|---|--------|-----------|---------------|-------------|
| 1 | `ticket_id` | String | `INC001` | Unique incident identifier (INC001–INC050) |
| 2 | `created_date` | Date (YYYY-MM-DD) | `2026-01-05` | Date the incident was logged |
| 3 | `severity` | Categorical | `P1`, `P2`, `P3` | Priority level — P1 is critical, P3 is low |
| 4 | `system` | Categorical | `SAP ECC`, `Oracle EBS` | Source ERP system |
| 5 | `module` | Categorical | `MM`, `FI`, `SD`, `BASIS`, etc. | ERP functional module affected |
| 6 | `issue_type` | Categorical | `Performance`, `Error`, `Configuration`, `Access` | Classification of the issue |
| 7 | `short_description` | Text | `SAP MM transaction MIGO extremely slow` | One-line summary of the incident |
| 8 | `detailed_description` | Text | `Users report MIGO transaction taking over 60 seconds...` | Full description of symptoms and user impact |
| 9 | `root_cause` | Text | `Database buffer pool overflow due to large batch posting...` | Identified root cause after investigation |
| 10 | `remediation_steps` | Text (numbered) | `1. Check ST02 buffer statistics 2. Increase buffer pool...` | Step-by-step resolution procedure |
| 11 | `assigned_agent` | Categorical | `Agent_DB_01`, `Agent_FI_01` | Agent or team who resolved the ticket |
| 12 | `resolution_time_hours` | Float | `4.5` | Hours from creation to resolution |
| 13 | `status` | Categorical | `Resolved` | Current ticket status (all are Resolved) |
| 14 | `business_impact` | Categorical | `Critical`, `High`, `Medium`, `Low` | Business impact assessment |
| 15 | `recurrence_count` | Integer | `12` | How many times this pattern has recurred historically |
| 16 | `environment` | Categorical | `Production` | Deployment environment (all are Production) |

---

## 4. Data Quality & Profile

### Coverage

| Dimension | Values |
|-----------|--------|
| **Total tickets** | 50 |
| **Date range** | 2026-01-05 to 2026-02-23 (50 days) |
| **Systems** | SAP ECC (38 tickets), Oracle EBS (12 tickets) |
| **Modules** | MM, FI, SD, BASIS, AP, GL, INV, PO, OM, WMS, FA, AR, HR (13 unique) |
| **Severity distribution** | P1: 16 tickets, P2: 18 tickets, P3: 16 tickets |
| **Issue types** | Performance (19), Error (18), Configuration (10), Access (3) |
| **Assigned agents** | 7 unique agents (Agent_DB_01, Agent_FI_01, Agent_SD_01, Agent_BASIS_01, Agent_MM_01, Agent_SEC_01, Agent_HR_01) |

### Key Statistics

| Metric | Value |
|--------|-------|
| **Avg resolution time** | ~3.8 hours |
| **Min resolution time** | 1.5 hours |
| **Max resolution time** | 12.0 hours |
| **Avg recurrence count** | ~7.5 |
| **Max recurrence count** | 18 (INC008 — Warehouse RF timeouts) |
| **Business impact: Critical** | 12 tickets |
| **Business impact: High** | 23 tickets |

### Data Realism

The dummy data is realistic for an enterprise AMS operation:

- **SAP transaction codes** are real (MIGO, FB01, VA02, ME51N, ST02, RZ10, OB52, etc.)
- **Oracle EBS programs** are real (APXIIMPT, AutoInvoice, Min-Max Planning, etc.)
- **Root causes** reflect actual production scenarios (buffer overflows, expired config, missing indexes, duplicate records)
- **Remediation steps** follow actual SAP/Oracle troubleshooting procedures
- **Agent names** follow a functional convention (DB, FI, SD, BASIS, MM, SEC, HR)

### Known Data Characteristics

- All tickets have `status = Resolved` — this is intentional since the clustering and KA generation work on historical resolved incidents.
- All tickets are in `environment = Production` — the MVP focuses on production operations.
- The `recurrence_count` field is the primary signal for identifying patterns worth automating.
- `remediation_steps` are semicolon-separated numbered steps in a single text field. The notebook parses these by splitting on `. ` (period + space) with a numbered prefix pattern.

---

## 5. Notebook Walkthrough — Agentic_AI_OPS_AMS_MVP.ipynb

The notebook contains **37 cells** (mix of Markdown headers and Python code) organized into 10 logical sections. Below is a detailed explanation of what each section does, why it matters, and the key design decisions.

---

### Section 0: Setup & Dependencies

**Cells 0–3** | Purpose: Environment preparation

**What it does:**

- Displays the project title and end-to-end workflow as a Markdown overview.
- Installs 8 Python packages if they are not already present: `pandas`, `numpy`, `scikit-learn`, `matplotlib`, `seaborn`, `networkx`, `plotly`, `tabulate`.
- Imports all required libraries and initializes the environment.

**Key design decision:** The install cell uses `try/except ImportError` to avoid reinstalling packages that already exist, making it safe to re-run. All packages are standard PyPI packages with no GPU or network-dependent model downloads, so it works on any Mac/PC or online Jupyter environment.

**Libraries by purpose:**

| Library | Used For |
|---------|----------|
| `pandas` | Data loading, manipulation, analysis |
| `numpy` | Numerical operations, random seed |
| `scikit-learn` | TF-IDF vectorization, KMeans clustering, PCA, scaling, cosine similarity |
| `matplotlib` + `seaborn` | Static charts (EKG graph, architecture diagram) |
| `plotly` | Interactive charts (cluster scatter, prioritization dashboard, executive KPIs) |
| `networkx` | Enterprise Knowledge Graph construction and analysis |
| `tabulate` | Table formatting |

---

### Section 1: Data Ingestion

**Cells 4–6** | Purpose: Load incident tickets and generate synthetic telemetry signals

**What it does:**

1. **Loads `dummy_incidents.csv`** — Searches a few common relative paths to find the file. Parses `created_date` as datetime. Prints summary statistics (count, date range, systems, modules, severity distribution). Displays the first 10 rows as a table.

2. **Generates synthetic OpenTelemetry signals** — Creates 3–8 signals per ticket (randomized), producing ~250+ signal records. Each signal has:
   - `ticket_id` — links back to the incident
   - `timestamp` — randomly 1–6 hours before the incident creation time
   - `signal_type` — one of: `metric.cpu_usage`, `metric.memory_usage`, `metric.response_time_ms`, `metric.error_rate`, `log.error`, `log.warning`, `trace.span_duration_ms`
   - `value` — realistic ranges (CPU: 75–99%, memory: 80–98%, response time: 2000–60000ms, etc.)
   - `service` — formatted as `{system}.{module}` (e.g., `SAP ECC.MM`)

**Why synthetic OTel data?** The PRD mentions "use Open Telemetry data." Since this is an MVP with no live infrastructure, the notebook generates plausible signals that demonstrate how monitoring data would precede incident creation. In production, these would come from Prometheus, Grafana, Datadog, or an OpenTelemetry Collector.

---

### Section 2: Ticket Clustering

**Cells 7–10** | Purpose: Group similar incidents using NLP + ML

**What it does:**

1. **Feature engineering** — Combines multiple text fields into a single `combined_text` column per ticket:
   ```
   short_description | detailed_description | root_cause | issue_type | system module
   ```
   This gives the TF-IDF vectorizer rich context to work with.

2. **TF-IDF Vectorization** — Converts text into a 200-feature sparse matrix using unigrams and bigrams (e.g., "buffer pool", "posting period"). Stop words are removed. `min_df=1` ensures no terms are dropped in this small dataset.

3. **Categorical feature encoding** — Label-encodes `system`, `module`, `severity`, `issue_type` and includes `recurrence_count` as a numeric feature. These 5 features are scaled with `StandardScaler` and combined (via `scipy.sparse.hstack`) with the TF-IDF matrix, producing a combined feature matrix of shape ~(50, 205).

4. **Silhouette analysis** — Tests K values from 3 to 11, computing silhouette scores for each. Plots the scores and selects the K with the highest silhouette score as optimal. The red dashed line marks the chosen K.

5. **KMeans clustering** — Applies KMeans with the optimal K. Assigns a `cluster_id` to each ticket.

6. **Cluster naming** — Each cluster is auto-named by its dominant system, module, and issue type (e.g., `C0: SAP ECC-MM (Performance)`).

7. **PCA visualization** — Reduces the 205-dimensional feature space to 2D using PCA and renders an interactive Plotly scatter plot. Hovering over each point shows ticket ID, description, severity, system, and module.

**Why this approach?** Combining TF-IDF text features with categorical/numeric features ensures that clustering considers both the semantic content (what the ticket is about) and structured metadata (which system, how severe, how often). Pure text clustering would miss that an MM Performance ticket and an FI Performance ticket are structurally different despite similar vocabulary.

---

### Section 3: Cluster Prioritization

**Cells 11–13** | Purpose: Rank clusters to focus automation efforts

**What it does:**

1. **Computes per-cluster statistics:**
   - Ticket count
   - Average severity score (P1=4, P2=2.5, P3=1)
   - Total and average recurrence
   - Average and total resolution hours
   - Count of P1 tickets
   - Count of Critical-impact tickets
   - Top systems and modules

2. **Composite priority score** — Normalizes 5 metrics to 0–100 scale using `MinMaxScaler`, then computes a weighted sum:
   - Ticket count: 25%
   - Average severity: 25%
   - Total recurrence: 20%
   - Average resolution time: 15%
   - P1 count: 15%

   This produces a single `priority_score` (0–100) per cluster. Clusters are ranked by this score.

3. **Visualizations:**
   - Bar chart of priority scores by cluster
   - Bubble chart of ticket count vs. average resolution time (bubble size = total recurrence, color = priority score)

**Why composite scoring?** A single metric like "ticket count" is insufficient. A cluster with 5 P1-Critical tickets that each take 8 hours to resolve and recur 15 times is more important to automate than a cluster with 10 P3-Low tickets that take 2 hours each. The weighted composite score captures this multi-dimensional priority.

---

### Section 4: Knowledge Article (KA) Generation

**Cells 14–16** | Purpose: Automatically generate structured remediation guides

**What it does:**

1. **`KnowledgeArticleGenerator` class** — For each cluster, it:
   - Extracts the top 3 root causes by frequency
   - Parses remediation steps from all tickets in the cluster, counts step frequency, and selects the top 8 most common steps
   - Identifies which agents resolved tickets in this cluster
   - Produces a structured KA dictionary containing:
     - **KA ID** (e.g., `KA-000`, `KA-001`)
     - **Version** (starts at 1.0 — supports version control)
     - **RCA summary** (primary causes, affected systems/modules)
     - **Remediation steps** (ordered, with frequency counts)
     - **Suggested agents** (with ticket counts)
     - **Validation steps** (4 standard post-fix checks)
     - **Automation potential** (High/Medium/Low based on total recurrence threshold)
     - **Estimated MTTR reduction** (percentage, derived from recurrence)

2. **Generates all KAs** — One per cluster, stored in an in-memory `ka_store` dictionary.

3. **Displays all KAs** — Renders each as formatted Markdown, sorted by priority score. Each KA includes the RCA summary, numbered remediation steps, agent assignments, and validation steps.

**Why this matters:** In the PRD, KAs serve as "structured prompts and algorithmic guidance for the AI-OPS Brain." They transform raw incident data into actionable, version-controlled remediation playbooks. In production, these would be stored in a database and continuously refined.

---

### Section 5: Enterprise Knowledge Graph (EKG)

**Cells 17–20** | Purpose: Build and analyze a directed graph of all relationships

**What it does:**

1. **Graph construction** using `networkx.DiGraph()`. For each incident ticket, it creates nodes and edges for:

   | Node Type | Example | Color |
   |-----------|---------|-------|
   | System | `SYS:SAP ECC` | Red |
   | Module | `MOD:SAP ECC.MM` | Teal |
   | Issue Type | `ISS:Performance` | Blue |
   | Root Cause | `RC:a3f8b2c1` (MD5 hash ID) | Salmon |
   | Agent | `AGT:Agent_DB_01` | Green |
   | Cluster | `CL:0` | Plum |
   | Knowledge Article | `KA:KA-000` | Gold |
   | Signal | `SIG:metric.cpu_usage` | Light Blue |

   Edges capture relationships like:
   - `has_module` (system → module)
   - `experiences_issue` (module → issue type, weighted by recurrence)
   - `caused_by` (issue → root cause)
   - `resolved_by` (root cause → agent)
   - `belongs_to_cluster` (root cause → cluster)
   - `documented_in` (cluster → KA)
   - `emits_signal` (module → signal type)

2. **Visualization** — Renders the full EKG using `spring_layout` with color-coded nodes and a legend. Root cause nodes show truncated labels (60 chars). Important node types get labels; root causes are shown as smaller dots to reduce clutter.

3. **EKG analytics:**
   - **Graph density** — How interconnected the graph is
   - **Top 10 hub nodes** by degree centrality — Identifies the most connected entities (e.g., a module that experiences many issue types)
   - **Sample paths** — Finds shortest paths from System nodes to KA nodes, demonstrating the graph traversal: `SAP ECC → SAP ECC.MM → Performance → [root cause] → [cluster] → KA-000`

**Why a Knowledge Graph?** The EKG is the "connective tissue" of the system. It enables the AI-OPS Brain to reason about relationships: "If Module X emits signal Y and has issue type Z, then root cause R is likely, and KA-K has the remediation." It supports decision tree generation and future graph-based machine learning (e.g., link prediction for new issue types).

---

### Section 6: Vector Store & Semantic Search

**Cells 21–23** | Purpose: Enable instant KA retrieval for new incidents

**What it does:**

1. **`KAVectorStore` class** — A lightweight vector database replacement that uses TF-IDF + cosine similarity:
   - `add()` — Inserts documents (KA text representations) with metadata and IDs
   - `build_index()` — Fits a TF-IDF vectorizer on all documents and stores the sparse matrix
   - `query()` — Transforms a query string into the same TF-IDF space, computes cosine similarity against all stored documents, and returns the top-N matches with similarity scores (expressed as distances: `1 - similarity`)
   - `count()` — Returns total document count

2. **Populates the store** — Each KA is converted into a text string combining title, systems, modules, root causes, and top remediation steps. This text is indexed.

3. **Semantic search demo** — Tests 5 simulated new incident descriptions against the store:
   - `"SAP system performance degraded, all transactions slow..."` → matches the performance cluster KA
   - `"Oracle AP invoice import failing with duplicate error..."` → matches the Oracle AP cluster KA
   - `"Users cannot post financial documents..."` → matches the FI configuration cluster KA
   - `"Warehouse RF scanners timing out..."` → matches the MM/WM performance cluster KA
   - `"Background job queue full..."` → matches the BASIS performance cluster KA

   For each, it shows the matched KA ID, confidence percentage, automation potential, and ticket count.

**Why not ChromaDB/Pinecone?** The PRD says "keep the code simple, we are building MVP not production product yet." A TF-IDF cosine similarity store has zero external dependencies, runs anywhere, and demonstrates the exact same retrieval pattern. The class API is designed as a drop-in replacement — swapping to ChromaDB in production requires changing only the class implementation, not the calling code.

---

### Section 7: Agent & Remediation Enablement

**Cells 24–26** | Purpose: Define agents and generate structured workflows

**What it does:**

1. **`AgentRegistry` class** — Defines 7 AI agents with:

   | Agent | Name | Autonomy Level | Risk Tolerance | Key Capabilities |
   |-------|------|---------------|----------------|------------------|
   | `Agent_DB_01` | Database Performance Agent | Semi-Autonomous | Medium | Index optimization, buffer tuning, query analysis, lock resolution |
   | `Agent_FI_01` | Financial Operations Agent | Human-Supervised | Low | Period management, payment processing, tax configuration |
   | `Agent_SD_01` | Sales & Distribution Agent | Semi-Autonomous | Medium | Pricing config, output determination, credit management |
   | `Agent_BASIS_01` | Basis & Infrastructure Agent | Autonomous | Medium | Job scheduling, transport management, RFC monitoring |
   | `Agent_MM_01` | Materials Management Agent | Semi-Autonomous | Medium | PO management, inventory control, batch management |
   | `Agent_SEC_01` | Security & Access Agent | Human-Supervised | Low | User management, role assignment, auth troubleshooting |
   | `Agent_HR_01` | HR Operations Agent | Human-Supervised | Low | Absence management, payroll support |

   Autonomy levels reflect real-world risk: financial and security operations require human oversight; infrastructure monitoring can be more autonomous.

2. **`RemediationWorkflow` class** — For each KA, generates a step-by-step workflow:
   - Each remediation step is classified as:
     - **Automated** — if the action involves checking, monitoring, running, analyzing, querying, or verifying (and does not require approval)
     - **Requires Approval** — if the action involves updating, changing, modifying, deleting, restarting, reversing, or mass operations
     - **Semi-Automated** — everything else
   - Risk level is tagged (High for approval-required steps, Low otherwise)
   - Estimated time per step is assigned (2–15 minutes, randomized for the demo)
   - KA validation steps are appended as automated validation actions
   - Workflow is rendered with color-coded rows (green = automated, yellow = semi-automated, red = requires approval)
   - Automation coverage percentage is calculated per workflow

**Why classify execution modes?** In production AMS, not every step should be autonomous. Modifying configuration (e.g., "Update posting period in OB52") carries risk and needs approval. Reading/checking (e.g., "Check ST02 buffer statistics") is safe to automate. This classification directly feeds the AI-OPS Brain's execution strategy.

---

### Section 8: AI-OPS Brain — Plan Generation & Execution

**Cells 27–30** | Purpose: Central orchestrator that ties everything together

**What it does:**

1. **`AIOPSBrain` class** — The orchestrator that takes in:
   - The EKG (NetworkX graph)
   - The KA store (all Knowledge Articles)
   - The Agent Registry
   - The Vector Store

   **`analyze_incident()`** method:
   - Step 1: Queries the vector store with the incident description to find the best-matching KA
   - Step 2: Determines execution strategy based on confidence and automation potential:
     - **AUTONOMOUS** — confidence > 60% AND automation potential is High or Medium
     - **SUPERVISED** — confidence > 35%
     - **HUMAN_ESCALATION** — confidence ≤ 35%
   - Step 3: Builds an execution plan with:
     - Plan ID (timestamped)
     - Matched KA and confidence score
     - List of steps with assigned agents and estimated times
     - Risk controls (pre-execution backup, rollback plan, change approval, post-validation, 24hr monitoring)
     - Estimated total resolution time

   **`execute_plan()`** method:
   - Simulates executing each step with a 95% success probability
   - Failed steps get one retry (80% success on retry)
   - Prints a formatted execution log showing step number, status (SUCCESS/RETRY/ESCALATED), agent, time, and action
   - Displays risk control checklist
   - Logs execution results for future analysis

2. **Demo: First incident** — Simulates receiving:
   > "SAP system extremely slow, dialog response times over 5 seconds, users complaining about MM and SD transactions timing out. HANA memory utilization at 95%. Background jobs are queueing up."

   The Brain matches this to the performance cluster KA, generates an AUTONOMOUS or SUPERVISED plan, and executes it step by step.

3. **Demo: Second incident** — Simulates receiving:
   > "Oracle AP invoice interface rejecting 200+ invoices with unique constraint violation. Upstream system sending duplicate invoice numbers."

   Demonstrates that the Brain handles both SAP and Oracle incidents and selects the correct KA.

**Why this architecture?** The Brain's three-tier strategy (Autonomous/Supervised/Escalation) mirrors how real AMS operations work. High-confidence matches with well-documented solutions can be automated. Low-confidence matches need human review. The risk controls ensure safety — no changes happen without a rollback plan.

---

### Section 9: Executive Dashboard

**Cells 31–33** | Purpose: Visualize KPIs and business outcomes

**What it does:**

1. **Before/after metrics** — Uses simulated monthly data from Jul 2025 to Mar 2026 (AI-OPS launch in Dec 2025):

   | Metric | Before AI-OPS | After AI-OPS | Improvement |
   |--------|--------------|--------------|-------------|
   | Monthly incidents | 155 | 72 | 53% reduction |
   | MTTR (hours) | 5.9 | 2.1 | 64% reduction |
   | Autonomous resolution | 10% | 65% | +55 percentage points |
   | Human agent utilization | 92% | 58% | -34 percentage points |

2. **Interactive Plotly dashboard** with 4 charts:
   - Bar chart: Monthly incident volume (blue = before, green = after)
   - Line chart: MTTR trend
   - Bar chart: Autonomous resolution percentage
   - Area chart: Human agent utilization

   A red dashed line marks the AI-OPS launch date.

3. **Automation readiness table** — For each cluster, shows:
   - Number of tickets and recurrence
   - Percentage of workflow steps that are automated
   - Priority score
   - Readiness classification (Ready / Partial / Manual)

---

### Section 10: Architecture Summary

**Cells 34–36** | Purpose: Visual architecture and final summary

**What it does:**

1. **Architecture diagram** — Renders a matplotlib-based box-and-arrow diagram showing the flow:
   ```
   Data Sources → Ticket Clustering → Prioritization → KA Generation
        ↓                                                    ↓
   Enterprise Knowledge Graph ←→ AI-OPS Brain ←→ Agent Registry
                                    ↓
                              OUTCOMES
   ```

2. **Module status table** — Summarizes all 10 components and their implementation status (all Complete).

3. **Production roadmap** — Lists the next steps for moving from MVP to production.

---

## 6. Technology Stack

| Category | Technology | Purpose in This MVP |
|----------|-----------|---------------------|
| **Language** | Python 3.x | Entire implementation |
| **Environment** | Jupyter Notebook | Interactive development and demo |
| **Data** | Pandas, NumPy | Data loading, manipulation, statistics |
| **NLP/ML** | scikit-learn (TF-IDF, KMeans, PCA, cosine similarity) | Text vectorization, clustering, dimensionality reduction, semantic search |
| **Graph** | NetworkX | Enterprise Knowledge Graph construction and traversal |
| **Visualization** | Plotly (interactive), Matplotlib + Seaborn (static) | Charts, dashboards, EKG visualization, architecture diagram |
| **Data Format** | CSV | Incident ticket storage |

### What is NOT included (MVP scope)

- No LLM/GPT integration (KA generation is rule-based, not generative)
- No persistent database (all in-memory)
- No real vector database (ChromaDB/Pinecone replaced by TF-IDF cosine similarity)
- No real agent execution (SSH/API calls to SAP/Oracle)
- No authentication, RBAC, or audit logging
- No web UI (Jupyter is the UI)

---

## 7. How to Run the Demo

### Prerequisites

- Python 3.8+ installed
- Jupyter Notebook or JupyterLab installed (or use Google Colab / VS Code Jupyter extension)
- Internet access for initial `pip install` (only on first run)

### Steps

1. **Place both files in the same directory:**
   ```
   your-folder/
   ├── Agentic_AI_OPS_AMS_MVP.ipynb
   └── dummy_incidents.csv
   ```

2. **Start Jupyter:**
   ```bash
   jupyter notebook
   ```
   Or open the `.ipynb` file in VS Code, Google Colab, or any Jupyter-compatible environment.

3. **Run all cells sequentially:**
   - Use `Kernel > Restart & Run All` or `Cell > Run All`
   - The first cell will install any missing packages automatically
   - Total execution time: approximately 30–60 seconds on a standard laptop

4. **Interact with outputs:**
   - Plotly charts are interactive — hover for details, zoom, pan
   - Scroll through Knowledge Articles to see per-cluster remediation guides
   - The AI-OPS Brain demo shows two end-to-end incident resolution simulations

### Troubleshooting

| Issue | Solution |
|-------|----------|
| `FileNotFoundError: dummy_incidents.csv` | Ensure the CSV is in the same folder as the notebook |
| `ModuleNotFoundError` | Re-run the first code cell (cell 2) to install dependencies |
| Plotly charts not rendering | Install `jupyter-plotly` extension or use JupyterLab |
| Slow silhouette analysis | Normal — it tests 9 different K values; takes ~5 seconds |

---

## 8. Architecture Diagram (Text)

```
┌─────────────────────────────────────────────────────────────────┐
│                     DATA SOURCES                                 │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐          │
│  │ SAP/Oracle   │  │ OpenTelemetry│  │ CSV/Excel/   │          │
│  │ Tickets      │  │ Signals      │  │ APIs         │          │
│  └──────┬───────┘  └──────┬───────┘  └──────┬───────┘          │
└─────────┼─────────────────┼─────────────────┼───────────────────┘
          └─────────────────┼─────────────────┘
                            ▼
┌─────────────────────────────────────────────────────────────────┐
│  TICKET CLUSTERING                                               │
│  TF-IDF Vectorization + KMeans + Silhouette Optimization        │
│  Input: 50 tickets → Output: K clusters with names              │
└───────────────────────────┬─────────────────────────────────────┘
                            ▼
┌─────────────────────────────────────────────────────────────────┐
│  CLUSTER PRIORITIZATION                                          │
│  Composite Score = 0.25×Volume + 0.25×Severity + 0.20×Recur    │
│                  + 0.15×MTTR + 0.15×P1Count                     │
└───────────────────────────┬─────────────────────────────────────┘
                            ▼
┌─────────────────────────────────────────────────────────────────┐
│  KNOWLEDGE ARTICLE GENERATION                                    │
│  Per cluster: RCA summary, remediation steps, agents, validation│
│  Version-controlled, stored in KA Store                         │
└──────────┬────────────────┬─────────────────────────────────────┘
           ▼                ▼
┌──────────────────┐  ┌──────────────────────────────────────────┐
│  ENTERPRISE       │  │  VECTOR STORE (TF-IDF Cosine Sim)       │
│  KNOWLEDGE GRAPH  │  │  Semantic search for KA matching        │
│  NetworkX DiGraph │  │  Query: new incident → best KA          │
│  ~100+ nodes      │  └──────────────────┬──────────────────────┘
│  8 relationship   │                     │
│  types            │                     │
└──────────┬───────┘                     │
           └──────────┬──────────────────┘
                      ▼
┌─────────────────────────────────────────────────────────────────┐
│  AI-OPS BRAIN (ORCHESTRATOR)                                     │
│  1. Match incident → KA via vector search                       │
│  2. Determine strategy: Autonomous / Supervised / Escalation    │
│  3. Generate execution plan with risk controls                  │
│  4. Execute steps, monitor, adjust                              │
└───────────────────────────┬─────────────────────────────────────┘
                            ▼
┌─────────────────────────────────────────────────────────────────┐
│  AGENT REGISTRY & REMEDIATION WORKFLOWS                          │
│  7 AI Agents with capabilities, autonomy levels, risk tolerance │
│  Color-coded workflows: Green=Auto, Yellow=Semi, Red=Approval   │
└───────────────────────────┬─────────────────────────────────────┘
                            ▼
┌─────────────────────────────────────────────────────────────────┐
│  OUTCOMES                                                        │
│  53% fewer incidents │ 64% faster MTTR │ 65% autonomous         │
└─────────────────────────────────────────────────────────────────┘
```

---

## 9. Production Roadmap

| Phase | Enhancement | Technology |
|-------|------------|------------|
| **Phase 1** | Connect real ITSM data | ServiceNow / SAP SolMan / Jira REST APIs |
| **Phase 2** | Add LLM for richer KA generation | GPT-4 / Claude / Azure OpenAI |
| **Phase 3** | Persistent storage | PostgreSQL + Neo4j for EKG, ChromaDB/Pinecone for vectors |
| **Phase 4** | Real agent execution | SSH/API connectors to SAP (RFC), Oracle (REST), cloud infra |
| **Phase 5** | CI/CD pipeline | Automated cluster retraining as new incidents arrive |
| **Phase 6** | Security & governance | RBAC, audit trail, approval workflows |
| **Phase 7** | OpenTelemetry integration | Real OTel Collector → correlation with incidents |
| **Phase 8** | Self-improving loop | Feedback from execution outcomes updates KAs and EKG automatically |

---

*Generated on 2026-04-02 | MVP Demo — Agentic AI-OPS Driven AMS*
