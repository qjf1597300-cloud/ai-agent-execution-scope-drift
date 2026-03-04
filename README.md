This repository documents a reproducible destructive command execution issue observed in an AI agent IDE environment.

# Execution Scope Drift in an AI Agent IDE

This repository documents a reproducible destructive filesystem incident observed while testing an AI-powered development environment.

A natural-language instruction asking the AI agent to remove a specific folder resulted in recursive deletion of an entire drive.

The command displayed to the user targeted a specific directory, but the execution scope expanded to the root of the drive.

Vendor support later confirmed that the issue was caused by a **systemic path-parsing failure**.

---

## Incident Summary

User intent:

Remove a `node_modules` folder under a project directory.

Example command shown to the user:

```
rmdir /s /q "E:\...\node_modules"
```

After confirmation, deletion began and the drive contents were removed recursively.

---

## Key Observation

Deletion began from:

```
$RECYCLE.BIN
```

This indicates that the command effectively executed on:

```
E:\
```

instead of the intended directory.

---

## Reproducibility

The behavior was reproduced multiple times using newly created directories.

The issue appeared to trigger when directory paths contained:

- spaces
- special characters such as `&`

Multiple recordings of the reproduction process were captured.

---

## Vendor Response

Vendor support confirmed the following:

> A technical review confirms that a systemic path-parsing failure caused the agent to execute a recursive `rmdir` command on the root directory.

The vendor also stated that similar failures had been observed previously and that guardrails and path validation were being implemented.

---

## Repository Structure

```
incident-report/     Detailed case study and technical analysis
evidence/            Screenshots and vendor correspondence
reproduction/        Reproduction notes and recordings
timeline/            Timeline of incident and investigation
```

---

## Purpose

This repository documents a potential safety issue in **AI agent execution environments**, where the command displayed to the user may differ from the scope of the command actually executed.

This phenomenon is referred to as:

**Execution Scope Drift**
