# CTSE MAS - Healthcare Multi-Agent System

SE4010 - Cloud Technologies and Software Engineering | Assignment 2

This repository implements a locally hosted Multi-Agent System (MAS) for healthcare triage and treatment planning. It covers the full assignment flow: patient intake, symptom analysis, treatment planning, and report generation.

---

## Assignment 2 Requirements (from CTSE - Assignment 2.pdf)

### Project Goal

Design, build, and run a local Multi-Agent System that solves a complex multi-step problem using autonomous agents, tools, and shared state.

### Technical Constraints (Zero-Cost and Local)

- Must run locally.
- Must use local SLMs via Ollama (no paid cloud LLM API keys).
- Must use an open-source orchestrator (LangGraph, CrewAI, or AutoGen).

### Required Architecture Components

1. Multi-agent orchestration with at least 3-4 distinct agents.
2. Custom Python tools used by agents.
3. Secure global state handoff between agents.
4. LLMOps/AgentOps observability (inputs, tool calls, outputs, traces).

### Individual Student Requirements

Each student must:

1. Build one agent (prompt/constraints/persona).
2. Build one custom Python tool (with type hints and docstrings).
3. Contribute automated testing/evaluation for that agent.

### Deliverables

1. Source repository with MAS implementation, agents, tools, tests.
2. Demo video (4-5 minutes, not above 5).
3. Technical report (4-8 pages, not above 8) including architecture, tools, state management, evaluation, and contribution proof.

---

## Problem Domain

Healthcare - automated patient intake, diagnosis support, treatment planning, and medical report generation.

Pipeline:

1. Load and validate patient data.
2. Analyze symptoms and produce differential diagnosis.
3. Generate allergy-safe treatment recommendations.
4. Generate final medical report and structured artifacts.

---

## Team Members and Responsibilities

| Student ID | Name | Agent | Tool |
| ---------- | ---- | ----- | ---- |
| IT22248244 | Pandithasundara N B | PatientIntakeAgent | tool_patient_reader |
| IT22014290 | Samishka H T | SymptomAnalyzerAgent | tool_symptom_analyzer |
| IT22333148 | Wijerathne C G T N | TreatmentPlannerAgent | tool_medication_recommender |
| All members | All members | MedicalReportAgent | tool_report_generator |

---

## System Architecture

```text
Patient JSON File
        |
        +-------------------------------------+
        |                                     |
        v                                     v
    main.py                          main_langgraph.py
 (Sequential)                        (LangGraph DAG)
        |                                     |
        v                                     v
PatientIntakeAgent ----------------> intake node
        |
        v
SymptomAnalyzerAgent --------------> symptom node
        |
        v
TreatmentPlannerAgent -------------> treatment node
        |
        v
MedicalReportAgent -----------------> report node
        |
        v
reports/*.md + reports/*.json + logs/trace_*.json
```

Orchestration modes:

- Sequential orchestrator: main.py
- LangGraph orchestrator: main_langgraph.py

---

## Assignment Compliance Mapping

### 1) Multi-Agent Orchestration (3-4 agents)

- Agents are implemented in:
  - agents/agent_patient_intake.py
  - agents/agent_symptom_analyzer.py
  - agents/agent_treatment_planner.py
  - agents/agent_report_generator.py
- Sequential orchestration: main.py
- LangGraph orchestration: main_langgraph.py

### 2) Tool Usage (custom Python tools)

- tools/tool_patient_reader.py
- tools/tool_symptom_analyzer.py
- tools/tool_medication_recommender.py
- tools/tool_report_generator.py

### 3) State Management

- Shared state model: config/state.py
- State is passed and enriched across all agents.

### 4) Observability / Tracing

- Logging and trace utilities: config/observability.py
- Execution traces generated in logs/trace_*.json
- Pipeline outputs generated in reports/*.md and reports/*.json

### 5) Individual Agent Testing and Group Integration

- Per-agent tests:
  - tests/test_agent1_patient_intake.py
  - tests/test_agent2_symptom_analyzer.py
  - tests/test_agent3_treatment_planner.py
  - tests/test_agent4_report_generator.py
- End-to-end integration:
  - tests/test_pipeline_integration.py

---

## Project Structure

```text
.
├── README.md
├── main.py
├── main_langgraph.py
├── requirements.txt
├── conftest.py
├── agents/
├── tools/
├── config/
├── data/
├── tests/
├── reports/
└── logs/
```

---

## Setup and Installation

### Prerequisites

- Python 3.9+ (3.10+ recommended)
- Ollama (required by assignment for local SLM reasoning)

### Install Dependencies

```bash
python3 -m venv .venv
source .venv/bin/activate
python3 -m pip install -r requirements.txt
```

### Ollama Setup (Assignment Requirement)

```bash
ollama pull llama3.2:3b
ollama serve
```

Note: This codebase is fault-tolerant and still runs if Ollama is unavailable, but assignment demonstrations should include local SLM-enabled execution.

---

## Run the System

```bash
# Sequential pipeline
python3 main.py --patient data/patients/patient_PT001.json

# LangGraph pipeline
python3 main_langgraph.py --patient data/patients/patient_PT001.json
```

---

## Run Tests

```bash
python3 -m pytest tests -v
```

---

## Output Artifacts

- Markdown reports: reports/*.md
- Structured report exports: reports/*.json
- Execution traces: logs/trace_*.json

---

## Submission Checklist (Assignment 2)

1. Repository contains 3-4 agents, tools, orchestration, and tests.
2. Demo video prepared (4-5 minutes, max 5).
3. Technical report prepared (4-8 pages, max 8) with:
   - Problem definition
   - Architecture and workflow diagram
   - Agent design and interaction strategy
   - Tool descriptions and examples
   - State management design
   - Testing and evaluation methodology
   - Contribution proof per student
   - Repository link
