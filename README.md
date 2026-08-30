# Banking GenAI Automated UAT & Release Guardrail Pipeline

An enterprise-grade, automated User Acceptance Testing (UAT) evaluation pipeline that validates customer-facing conversational banking AI against official bank fee schedules, Central Bank of the UAE (CBUAE) consumer protection rules, and brand safety policies prior to production release.

---

## Executive Summary

Deploying Generative AI across digital banking apps introduces serious compliance and financial risks:
* **Hallucinated Fees & Rates:** Quoting outdated or fabricated interest rates violates CBUAE Consumer Protection Regulations (CPR).
* **Manual UAT Bottlenecks:** Manually testing hundreds of prompt variations requires weeks of cross-functional review across Product, QA, and Compliance teams.
* **Competitor Defection & Brand Risk:** Unaligned LLMs can recommend competitor products or omit statutory cooling-off disclosures.

This platform automates regression testing into an **LLM-as-a-Judge deployment gate**. Model releases are automatically graded against a codified 1–5 scoring rubric. Any critical rate hallucination or policy breach triggers a hard release freeze and generates structured Jira bug tickets with Gherkin acceptance criteria.

---

## Evaluation Pipeline Architecture

```text
[Engineering Updates Prompt/Model] 
                 │
                 ▼
[Automated Test Runner Executes Golden Dataset]
                 │
                 ▼
   [LLM-as-a-Judge Evaluation Engine]
  (Audits against Fee Specs & CBUAE CPR)
                 │
        ┌────────┴────────┐
        ▼                 ▼
[Criteria Failed ❌]   [Criteria Passed ✅]
(Score ≤ 2 / Breaches) (Groundedness ≥ 95% & No Blockers)
        │                 │
        ▼                 ▼
[🛑 DEPLOYMENT BLOCKED] [🚀 RELEASE APPROVED]
        │
        ▼
[Automated Gherkin Jira Tickets Generated]
```

---

## Evaluation Rubric & Release Gates

| Score | Classification | Description | Deployment Action |
|---|---|---|---|
| **5** | **Perfect Pass** | 100% accurate figures, exact schedule match, complete statutory disclosures | ✅ Approved |
| **4** | **Pass** | Accurate pricing and compliance terms with minor stylistic variance | ✅ Approved |
| **3** | **Needs Review** | Partially correct info, ambiguous phrasing, or non-critical omission | ⚠️ Review Required |
| **2** | **Fail** | Outdated rates, incorrect calculations, or missing required terms | 🛑 Freeze Release |
| **1** | **Hard Blocker** | Hallucinated interest/fees, competitor recommendations, CBUAE regulatory violations | 🛑 Hard Freeze + Jira Bug |

---

## Key Features

1. **Self-Serve Dataset Management:**
   - Pre-loaded golden UAT banking dataset (`data/golden_uat_bank.json`).
   - Drag-and-drop support for custom CSV / JSON test banks.
   - Live interactive table editor with direct JSON export.
2. **Automated Batch Release Gate:**
   - Configurable minimum groundedness threshold slider (80%–100%).
   - Dynamic Gemini Model routing (`gemini-2.5-flash`, `gemini-2.0-flash`, `gemini-1.5-flash`, `gemini-1.5-pro`, etc.).
   - Visual distribution bar charts and one-click CSV audit report export.
3. **Ad-Hoc Query Sandbox:**
   - Single-query tester for rapid prompt iteration, instant live grading, and hallucination audits.
4. **Jira Bug & Backlog Generator:**
   - Converts failed test scenarios into ready-to-use Jira tickets containing Gherkin BDD scenarios and exact remediation instructions.

---

## Project Structure

```
genai-uat-compliance-pipeline/
├── app.py                     # Streamlit frontend & evaluation pipeline
├── requirements.txt           # Python dependencies
├── data/
│   └── golden_uat_bank.json  # Reference bank golden test cases
├── docs/                      # Documentation assets
├── .streamlit/                # Streamlit configuration
└── README.md                  # Project overview & guide
```

---

## Quick Start

### 1. Clone & Setup Environment
```bash
git clone https://github.com/RaghavendranRThejes/genai-uat-compliance-pipeline.git
cd genai-uat-compliance-pipeline

python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

### 2. Launch the Application
```bash
streamlit run app.py
```

### 3. Run Evaluations
1. Open the application at `http://localhost:8501`.
2. Input your **Gemini API Key** in the sidebar.
3. Navigate to **Tab 2 (Automated Batch Release Pipeline)** and click **"Run Automated UAT Evaluation Suite"**.
4. Inspect audit findings, pass/fail metrics, and auto-generated Jira user stories.
