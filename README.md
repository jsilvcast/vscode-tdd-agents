# 🤖 VSCODE AGENTS 🤖

This repository focus on TDD agents for Python projects, providing a structured workflow for writing tests, implementing features, and refactoring code while maintaining high quality standards.

## Changelog
- v0.0.1 - Initial commit with TDD agents for Red, Green, and Refactor phases following the TDD cycle based on lineal chain (Red -> Green -> Refactor -> Red).
- v1.0.0 - Added detailed workflows for each TDD phase, including tool usage and shared state management via `.tdd_memory.md`. 
- v2.0.0 - Planning support (multi-task loops), TDD Docs agent and standalone Debugger agent
- v2.1.0 - History log: all agents append state transitions to `.tdd_history.json` for full audit trail.

## Agents Overview (as of v2.1.0)
### TDD Supervisor Agent
- Orchestrates the TDD cycle, ensuring proper handoffs between Red, Green, Refactor, and Docs agents based on the current status and plan in `.tdd_memory.md`.
- Populates the `## Plan` table when the user provides a feature request, breaking it into tasks.
- Automatically loops through tasks: after completing one (Red→Green→Refactor→Docs), it picks the next `TODO` task from the plan.
### TDD Red Agent
- Writes failing tests based on the current `IN_PROGRESS` task from the plan.
### TDD Green Agent
- Implements minimal code to make the test pass while ensuring code quality.
### TDD Refactor Agent
- Refactors code while maintaining passing tests and strict linting standards.
### TDD Docs Agent
- Writes or updates `README.md` and `docs/` when the plan marks a task for documentation.
- Invoked by the Supervisor only when `Docs?: Yes` for the current task.
### Debugger Agent
- **Independent** of the TDD cycle (no handoffs to the Supervisor).
- Uses Claude Opus 4.6 for deep analysis of complex bugs, runtime errors, and logic issues.
- Can be invoked directly by the user at any time.
### Shared State Management
- All TDD agents read from and write to `.tdd_memory.md` to maintain a single source of truth regarding the current status, features, errors, modified files, linting status, and the task plan.

### History Log (`.tdd_history.json`)
- Every agent appends a JSON entry each time it changes the TDD status.
- Provides a full audit trail: who did what, when, on which task, and which files were touched.
- Useful for reviewing TDD sessions, debugging agent behavior, and team retrospectives.

## Pre-requisites
Property `chat.customAgentInSubagent.enabled` must be set to true in vscode settings to allow agents to call other agents as part of the TDD cycle.

## Usage
You only need to interact with the TDD Supervisor agent. It will manage the workflow and handoffs between the Red, Green, and Refactor agents based on the current status in `.tdd_memory.md`.

Sample prompts to the TDD Supervisor:
- "I want to start a new feature for PDF processing (splitting and OCR). Please guide me through the TDD cycle."

## Troubleshooting
If you get this message: 

```
Copilot has been working on this problem for a while. It can continue to iterate, or you can send a new message to refine your prompt. Configure max requests.
```

You will need to set chat.agent.maxRequests to a higher number in your vscode settings.