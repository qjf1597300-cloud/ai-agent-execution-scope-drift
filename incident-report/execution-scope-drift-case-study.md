# Execution Scope Drift in an AI Agent IDE

## Overview

This document reconstructs an incident where an AI agent executing system commands deleted an entire drive instead of the intended folder.

The issue occurred in an AI-powered IDE environment where the agent generated and executed a destructive filesystem command.

## Intended Command

The command displayed to the user targeted a specific folder:

```
rmdir /s /q "E:\...\node_modules"
```

The user confirmed the command through the IDE interface.

## Observed Result

Instead of removing the target folder, the system began deleting the entire drive.

The first directory observed during deletion was:

```
$RECYCLE.BIN
```

This indicates that the effective deletion scope became the drive root.

## Vendor Confirmation

Vendor support later confirmed that the issue was caused by:

> a systemic path-parsing failure

Support also indicated that this failure mode had been observed previously.

## Reproducibility

The behavior was reproduced multiple times in newly created directories.

The issue appears to trigger when directory paths contain:

- spaces
- special characters such as `&`

## Safety Implications

This case suggests a potential failure mode in AI agent execution environments where the displayed command may not match the scope of the executed command.

This phenomenon is referred to as:

**Execution Scope Drift**

##**See evidence/ for supporting screenshots.**
