# CTSE MAS — Multi-Agent Software Analysis System
**SE4010 – Cloud Technologies and Software Engineering | Assignment 2**

A locally-hosted **Multi-Agent System (MAS)** that automatically detects code quality bugs, security vulnerabilities, generates fixes, and produces a structured Markdown report — all without any cloud API calls.

---

## Team Members & Responsibilities

| Member | Name | Agent | Tool |
|--------|------|-------|------|
| Member 1 | Kavindi Perera | Code Analyzer Agent | `static_code_analyzer` |
| Member 2 | Ravindu Silva | Security Auditor Agent | `security_vulnerability_scanner` |
| Member 3 | Tharushi Fernando | Fix Suggester Agent | `automated_fix_suggester` |
| Member 4 | Dilshan Wickramasinghe | Report Generator Agent | `report_generator` |

---

## Problem Domain

**Software Engineering — Automated Code Review & Security Auditing**

Developers push vulnerable code daily. Manual code review is slow and inconsistent. This MAS automates the full cycle: detect → audit → fix → report, giving teams an instant security assessment of any Python file.

---

## System Architecture

```
Input File (Python source)
        │
        ▼
┌─────────────────────┐
│  CodeAnalyzerAgent  │  ← Tool: static_code_analyzer
│  (Member 1)         │    AST + regex bug detection
└────────┬────────────┘
         │ GlobalState (raw_bugs, metrics)
         ▼
┌──────────────────────────┐
│  SecurityAuditorAgent    │  ← Tool: security_vulnerability_scanner
│  (Member 2)              │    OWASP Top 10 + CVE mapping
└────────┬─────────────────┘
         │ GlobalState (security_issues, cve_references)
         ▼
┌──────────────────────┐
│  FixSuggesterAgent   │  ← Tool: automated_fix_suggester
│  (Member 3)          │    Rule-based code transformations
└────────┬─────────────┘
         │ GlobalState (suggested_fixes, patched_code)
         ▼
┌──────────────────────────┐
│  ReportGeneratorAgent    │  ← Tool: report_generator
│  (Member 4)              │    Markdown report + LLMOps trace
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
├── main.py                          # Pipeline entry point
├── sample_buggy_code.py             # Demo file with intentional flaws
├── requirements.txt
├── conftest.py
│
├── config/
│   ├── state.py                     # GlobalState + state management
│   └── observability.py             # LLMOps logging & tracing
│
├── agents/
│   ├── agent_code_analyzer.py       # Agent 1 (Member 1)
│   ├── agent_security_auditor.py    # Agent 2 (Member 2)
│   ├── agent_fix_suggester.py       # Agent 3 (Member 3)
│   └── agent_report_generator.py   # Agent 4 (Member 4)
│
├── tools/
│   ├── tool_static_analyzer.py      # Tool 1 (Member 1)
│   ├── tool_security_scanner.py     # Tool 2 (Member 2)
│   ├── tool_fix_suggester.py        # Tool 3 (Member 3)
│   └── tool_report_generator.py    # Tool 4 (Member 4)
│
├── tests/
│   ├── test_agent1_code_analyzer.py     # Member 1 tests (20 tests)
│   ├── test_agent2_security_auditor.py  # Member 2 tests (25 tests)
│   ├── test_agent3_fix_suggester.py     # Member 3 tests (21 tests)
│   ├── test_agent4_report_generator.py  # Member 4 tests (20 tests)
│   └── test_pipeline_integration.py    # Group unified harness (39 tests)
│
├── reports/                         # Generated Markdown reports
└── logs/                            # LLMOps JSON traces
```

---

## Setup & Installation

### Prerequisites
- Python 3.10+
- No paid API keys required

### Install Dependencies

```bash
pip install -r requirements.txt
```

### Run the Pipeline

```bash
# Analyze the sample buggy file
python main.py --file sample_buggy_code.py

# Analyze your own file
python main.py --file path/to/your_code.py
```

### Run All Tests

```bash
# All 125 tests
python -m pytest tests/ -v

# Individual member tests
python -m pytest tests/test_agent1_code_analyzer.py -v    # Member 1
python -m pytest tests/test_agent2_security_auditor.py -v # Member 2
python -m pytest tests/test_agent3_fix_suggester.py -v    # Member 3
python -m pytest tests/test_agent4_report_generator.py -v # Member 4

# Full pipeline integration test
python -m pytest tests/test_pipeline_integration.py -v
```

---

## What Gets Detected

### Code Quality Bugs (Agent 1)
- `SyntaxError` — invalid Python syntax
- `BareExcept` — bare `except:` clause
- `MutableDefaultArgument` — mutable list/dict as default arg
- `ZeroDivision` — division by zero literal
- `HardcodedCredential` — password/token in code
- `AssertInProduction` — assert used for runtime checks
- `MagicNumber` — unnamed numeric literals
- `TodoComment` — unresolved TODO/FIXME markers

### Security Vulnerabilities (Agent 2)
- `SQLInjection` — OWASP A03:2021
- `CodeInjection` — OWASP A03:2021
- `InsecureDeserialization` — OWASP A08:2021
- `WeakCryptography` — OWASP A02:2021
- `HardcodedSecret` — OWASP A02:2021
- `TLSVerificationDisabled` — OWASP A02:2021
- `InsecureRandom` — OWASP A02:2021
- `DebugModeEnabled` — OWASP A05:2021
- `UnvalidatedInput` — OWASP A01:2021

---

## Output Files

| File | Description |
|------|-------------|
| `reports/report_*.md` | Full Markdown security report |
| `logs/trace_*.json` | LLMOps execution trace (all agent actions, tool calls, timestamps) |

---

## Technical Requirements Met

| Requirement | Implementation |
|-------------|----------------|
| ✅ 4 Distinct Agents | CodeAnalyzer, SecurityAuditor, FixSuggester, ReportGenerator |
| ✅ Custom Python Tools | 4 tools with type hints, docstrings, error handling |
| ✅ State Management | `GlobalState` dataclass passed through all agents |
| ✅ LLMOps / Observability | `observability.py` logs every agent start/end/tool call + JSON trace |
| ✅ No Paid APIs | Runs entirely locally, no OpenAI/Anthropic keys |
| ✅ Individual Agent + Tool | Each member owns one agent and one tool |
| ✅ Testing & Evaluation | 125 tests: property-based, integration, and full pipeline |
