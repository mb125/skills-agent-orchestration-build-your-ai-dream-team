# Agent team

I will use **GitHub Copilot CLI in a Codespace** to orchestrate a custom four-agent team for **Mona's Project Pulse dashboard**. The Orchestrator coordinates the work, the Planner defines the implementation phases, the Designer shapes the dashboard UX and visual system, and the Coder implements the assigned files.

| Agent | Target model | Responsibility | Definition |
| --- | --- | --- | --- |
| Orchestrator | Claude Opus 4.7 (copilot) | Coordinates the project, requests a plan, breaks work into phases, delegates by file scope, and verifies the integrated result. | `.github/agents/orchestrator.agent.md` |
| Planner | Claude Opus 4.7 (copilot) | Researches the repository and produces the implementation plan, including steps, dependencies, parallel work, edge cases, and validation expectations. | `.github/agents/planner.agent.md` |
| Designer | Gemini 3.1 Pro (copilot) | Owns UI/UX, accessibility, information hierarchy, responsive behavior, and the polished dashboard presentation for Project Pulse. | `.github/agents/designer.agent.md` |
| Coder | GPT-5.5 (copilot) | Implements code, fixes bugs, adds runnable app support when assigned, and delivers deterministic, testable behavior within the assigned file scope. | `.github/agents/coder.agent.md` |
