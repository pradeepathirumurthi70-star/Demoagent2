# Agent team

GitHub Copilot CLI in a Codespace will orchestrate the custom agent team building
Mona's Project Pulse dashboard.

| Agent | Target model | Responsibility | Definition |
| --- | --- | --- | --- |
| Orchestrator | Claude Opus 4.7 (copilot) | Coordinates the Planner, Coder, and Designer; divides work into scoped phases, delegates tasks, and verifies the integrated result. | `.github/agents/orchestrator.agent.md` |
| Planner | Claude Opus 4.7 (copilot) | Researches the repository, documentation, dependencies, and edge cases, then produces an implementation plan. Does not write code. | `.github/agents/planner.agent.md` |
| Designer | Gemini 3.1 Pro (copilot) | Shapes the dashboard's user experience, visual design, accessibility, information hierarchy, interactions, and responsive behavior. | `.github/agents/designer.agent.md` |
| Coder | GPT-5.5 (copilot) | Implements assigned code and runnable-app support, follows repository patterns, and validates changes. | `.github/agents/coder.agent.md` |
