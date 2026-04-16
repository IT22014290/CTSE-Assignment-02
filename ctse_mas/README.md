# CTSE MAS — Healthcare Multi-Agent System
**SE4010 – Cloud Technologies and Software Engineering | Assignment 2**

A locally-hosted **Multi-Agent System (MAS)** that automates the full patient-care pipeline: intake, symptom analysis, treatment planning, and report generation — all without any cloud API calls.

---

## Team Members & Responsibilities

| Student ID | Name | Agent | Tool |
|------------|------|-------|------|
| IT22248244 | Pandithasundara N B | PatientIntakeAgent | `tool_patient_reader` |
| IT22014290 | Samishka H T | SymptomAnalyzerAgent | `tool_symptom_analyzer` |
| IT22333148 | Wijerathne C G T N | TreatmentPlannerAgent | `tool_medication_recommender` |
| All members | All members | MedicalReportAgent | `tool_report_generator` |

---

## Problem Domain

**Healthcare — Automated Patient Intake, Diagnosis & Treatment Pipeline**

Managing patient records, matching symptoms to conditions, and generating treatment plans are time-intensive tasks prone to human error. This MAS automates the full clinical workflow: load patient data → analyse symptoms → plan treatment → generate report, providing a consistent, structured medical assessment for any patient record.

---

## System Architecture

```
Patient JSON File
        │
        ▼
┌────────────────────────┐
│   PatientIntakeAgent   │  ← Tool: tool_patient_reader
│   (IT22248244)         │    Validates & loads patient record
└──────────┬─────────────┘
           │ GlobalState (patient_info, is_valid, validation_errors)
           ▼
┌────────────────────────────┐
│   SymptomAnalyzerAgent     │  ← Tool: tool_symptom_analyzer
│   (IT22014290)             │    Matches symptoms → probable conditions
└──────────┬─────────────────┘
           │ GlobalState (probable_conditions, confidence_scores)
           ▼
┌─────────────────────────────┐
│   TreatmentPlannerAgent     │  ← Tool: tool_medication_recommender
│   (IT22333148)              │    Allergy-screened medication plan
└──────────┬──────────────────┘
           │ GlobalState (treatment_plan, recommended_medications)
           ▼
┌──────────────────────────┐
│   MedicalReportAgent     │  ← Tool: tool_report_generator
│   (All members)                  │    Generates final Markdown report
└──────────────────────────┘
           │
           ▼
  reports/report_*.md  +  logs/trace_*.json
```

**Orchestration Pattern:** Sequential Coordinator-Worker pipeline. Each agent reads from and writes to the shared `GlobalState` object — no data is lost between handoffs.

---

## Project Structure

```
ctse_mas/
├── main.py                               # Pipeline entry point
├── sample_buggy_code.py                  # Demo patient record (buggy version)
├── requirements.txt
├── conftest.py
│
├── config/
│   ├── state.py                          # GlobalState dataclass + reset_state()
│   └── observability.py                  # LLMOps logging & Rich console tracing
│
├── agents/
│   ├── agent_patient_intake.py           # Agent 1 (IT22248244)
│   ├── agent_symptom_analyzer.py         # Agent 2 (IT22014290)
│   ├── agent_treatment_planner.py        # Agent 3 (IT22333148)
│   └── agent_report_generator.py         # Agent 4 (All)
│
├── tools/
│   ├── tool_patient_reader.py            # Tool 1 (IT22248244)
│   ├── tool_symptom_analyzer.py          # Tool 2 (IT22014290)
│   ├── tool_medication_recommender.py    # Tool 3 (IT22333148)
│   └── tool_report_generator.py          # Tool 4 (All)
│
├── data/
│   ├── symptoms_db.json                  # 10 medical conditions
│   ├── medications_db.json               # 14 medications
│   └── patients/
│       ├── patient_PT001.json            # Influenza case
│       └── patient_PT002.json            # UTI case (with comorbidities)
│
├── tests/
│   ├── test_agent1_patient_intake.py     # IT22248244 tests (21 tests)
│   ├── test_agent2_symptom_analyzer.py   # IT22014290 tests (22 tests)
│   ├── test_agent3_treatment_planner.py  # IT22333148 tests (21 tests)
│   ├── test_agent4_report_generator.py   # All tests (15 tests)
│   └── test_pipeline_integration.py     # Group integration harness (19 tests)
│
├── reports/                              # Generated Markdown reports
└── logs/                                 # LLMOps JSON traces
```

---

## Setup & Installation

### Prerequisites
- Python 3.10+
- No paid API keys required — runs 100% locally

### Create Virtual Environment & Install Dependencies

```bash
python3 -m venv .venv
source .venv/bin/activate      # Windows: .venv\Scripts\activate
pip install -r ctse_mas/requirements.txt
```

### Run the Pipeline

```bash
cd ctse_mas

# Process the default Influenza patient
python main.py --patient data/patients/patient_PT001.json

# Process the UTI patient (with comorbidities)
python main.py --patient data/patients/patient_PT002.json
```

### Run All Tests

```bash
cd ctse_mas

# Full test suite (98 tests)
python -m pytest tests/ -v

# Individual agent tests
python -m pytest tests/test_agent1_patient_intake.py -v     # IT22248244
python -m pytest tests/test_agent2_symptom_analyzer.py -v   # IT22014290
python -m pytest tests/test_agent3_treatment_planner.py -v  # IT22333148
python -m pytest tests/test_agent4_report_generator.py -v   # All members

# Full pipeline integration tests
python -m pytest tests/test_pipeline_integration.py -v
```

---

## What Each Agent Does

### Agent 1 — PatientIntakeAgent (IT22248244)
- Loads and validates the patient JSON record
- Checks required fields, age range, blood type, vital signs
- Populates `GlobalState` with patient demographics, symptoms, and vitals

### Agent 2 — SymptomAnalyzerAgent (IT22014290)
- Matches patient symptoms against `symptoms_db.json` (10 conditions)
- Scores and ranks probable conditions by confidence percentage
- Detected conditions include: Influenza, UTI, Pneumonia, COVID-19, Diabetes, and more

### Agent 3 — TreatmentPlannerAgent (IT22333148)
- Recommends medications from `medications_db.json` (14 medications)
- Screens against patient allergies before recommending
- Outputs a safe, structured treatment plan with dosage and duration

### Agent 4 — MedicalReportAgent (All)
- Aggregates all pipeline results from `GlobalState`
- Generates a structured Markdown report saved to `reports/`
- Records a full LLMOps execution trace to `logs/`

---

## Output Files

| File | Description |
|------|-------------|
| `reports/report_*.md` | Full patient medical report in Markdown |
| `logs/trace_*.json` | LLMOps execution trace (all agent actions, tool calls, timestamps) |

---

## Technical Requirements Met

| Requirement | Implementation |
|-------------|----------------|
| ✅ 4 Distinct Agents | PatientIntakeAgent, SymptomAnalyzerAgent, TreatmentPlannerAgent, MedicalReportAgent |
| ✅ Custom Python Tools | 4 tools with type hints, docstrings, and error handling |
| ✅ State Management | `GlobalState` dataclass passed by reference through all agents |
| ✅ LLMOps / Observability | `observability.py` logs every agent start/end/tool call + JSON trace |
| ✅ No Paid APIs | Runs entirely locally — no OpenAI/Anthropic keys needed |
| ✅ Individual Agent + Tool | Each member owns one agent and one corresponding tool |
| ✅ Testing & Evaluation | 98 tests: unit, integration, and full end-to-end pipeline |
