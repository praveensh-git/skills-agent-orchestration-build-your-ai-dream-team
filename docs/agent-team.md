# Agent team

I will use GitHub Copilot CLI in a Codespace to orchestrate Mona's Project Pulse dashboard team.

- **Orchestrator** — Claude Opus 5.5 (copilot). Coordinates the specialist agents, turns the implementation plan into phases, delegates work with clear file scopes, and verifies the integrated result. Its definition lives at `.github/agents/orchestrator.agent.md`.
- **Planner** — Claude Opus 5.5 (copilot). Researches the codebase and requirements, identifies dependencies and edge cases, and produces a practical implementation plan for the Orchestrator. Its definition lives at `.github/agents/planner.agent.md`.
- **Designer** — Gemini 3.7 Flash (copilot). Leads UI/UX, accessibility, responsive layout, information hierarchy, visual polish, and Project Pulse-specific dashboard styling. Its definition lives at `.github/agents/designer.agent.md`.
- **Coder** — GPT-5.5 (copilot). Implements the assigned code and app support files, follows repository patterns, and validates behavior and testability. Its definition lives at `.github/agents/coder.agent.md`.
