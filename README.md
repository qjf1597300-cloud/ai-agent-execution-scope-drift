# Execution Scope Drift in an AI Agent IDE

⚠️ Destructive command execution case study

This repository documents a reproducible filesystem deletion incident observed while testing an AI-powered development environment.

A natural-language instruction asking an AI agent to remove a specific folder resulted in recursive deletion of the entire drive.

The command displayed to the user targeted a specific directory, but the effective execution scope expanded to the root of the drive.

Vendor support later confirmed the cause as a **systemic path-parsing failure**.

---

# Example Command

The AI agent generated a command similar to:


rmdir /s /q "E:...\node_modules"


After confirmation, deletion began and the entire drive contents were removed recursively.

---

# Key Observation

Deletion started from:


$RECYCLE.BIN


Since this directory exists only at the root of a Windows drive, this strongly indicates that the effective deletion target became:


E:\


instead of the intended project directory.

---

# Execution Chain

The observed execution path was:


User instruction
↓
AI agent reasoning
↓
Command generation
↓
IDE execution wrapper
↓
PowerShell compatibility layer
↓
cmd /c
↓
rmdir
↓
Filesystem deletion


This multi-layer execution environment introduces multiple quoting and argument parsing stages.

---

# Reproducibility

The behavior was reproduced multiple times using newly created directories.

The issue appears to trigger when directory paths contain:

- spaces  
- special characters such as `&`

Multiple reproduction recordings were captured.

---

# Vendor Response

Vendor support confirmed the following in written correspondence:

> A technical review confirms that a systemic path-parsing failure caused the agent to execute a recursive `rmdir` command on the root directory.

The response also stated that this failure mode had been observed previously and that guardrails and path validation were being implemented.

---

# Full Technical Report

A detailed reconstruction of the incident, reproduction attempts, and technical analysis is available here:

➡️ `incident-report/execution-scope-drift-case-study.md`

---

# Purpose

This repository documents a potential safety issue in **AI agent execution environments**, where the command displayed to the user may differ from the scope of the command actually executed.

This phenomenon is referred to as:

**Execution Scope Drift**

---

# Repository Structure


incident-report/ Detailed technical case study
evidence/ Screenshots and vendor correspondence
reproduction/ Reproduction notes
timeline/ Incident timeline


---

# Notes

The incident and reproductions occurred on Antigravity IDE builds active between:


Late January 2026 – mid February 2026


Exact version numbers were not recorded.
