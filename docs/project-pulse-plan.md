# Project Pulse implementation plan

## Summary

Build a lightweight, responsive static dashboard that lets Mona's contributors see active projects, owners, status, recent activity, priority or risk, and a short summary at a glance. The app will load project records from JSON and be runnable from the VS Code **Run Project Pulse Dashboard** configuration. The `app/` directory does not exist yet, so the implementation creates it.

## Responsibilities

- **Designer** — Owns the visual direction and `app/styles.css`: establish readable hierarchy, responsive project cards, clear status and priority treatments, accessible contrast and focus states, and stable CSS hooks such as `.dashboard` and `.project-card`. Reviews the integrated UI for usability and accessibility without editing the Coder's files.
- **Coder** — Owns `app/project-data.json`, `app/index.html`, and `.vscode/launch.json`: provide deterministic sample records and rendering, integrate the data and stylesheet, handle loading/error/empty states, and configure a reliable local preview that opens the dashboard.
- **Orchestrator** — Coordinates phases and file boundaries, passes the data schema and CSS hooks between agents, integrates the deliverables, and checks the final dashboard against the brief.
- **Planner** — Defines implementation order, dependencies, risks, file ownership, and validation expectations.

## Ordered phases and file assignments

### 1. Establish independent design and app contracts

- **Designer — `app/styles.css`:** Create the responsive visual system and reusable card, status, priority, and layout styles. Document the class hooks the markup should use in the handoff.
- **Coder — `app/project-data.json`:** Create a top-level `projects` array with realistic, contributor-friendly sample projects. Each record must include `name`, `owner`, `status`, `recentActivity`, and `priority`; include a short `summary` so the dashboard can show the brief's contributor-friendly description.
- **Coder — `.vscode/launch.json`:** Add the **Run Project Pulse Dashboard** launch configuration. Use `${workspaceFolder}/app` as its working directory, serve the app from that directory, and open `/index.html` rather than a directory listing. Keep the configuration strict JSON and deterministic; do not require new project dependencies if a Codespace-provided server is sufficient.

These assignments can run in parallel: the stylesheet, data file, and launch configuration are separate files. The Designer should provide CSS hooks and the Coder should establish the data shape before the dashboard markup is integrated.

### 2. Integrate the dashboard page

- **Coder — `app/index.html`:** Build semantic, accessible dashboard markup that links `styles.css`, loads `project-data.json`, and renders the project records using the Designer's CSS hooks. Include visible project names, owners, statuses, recent activity, priority/risk, and summaries. Provide clear loading, empty, and data-load failure states.
- **Designer:** Review the rendered result against the visual and accessibility goals; report recommendations to the Coder without editing `index.html`.

This phase depends on the CSS hooks and data schema from Phase 1. Keep `index.html` under Coder ownership to avoid overlapping edits.

### 3. Integrate and validate

- **Orchestrator:** Verify the four assigned files work together, confirm the launch flow opens the dashboard, and route any focused fixes back to the file owner. The Coder owns implementation fixes; the Designer owns stylesheet adjustments.

This phase depends on Phases 1 and 2 completing.

## Dependencies and parallel work

- `app/index.html` depends on the `projects` data shape in `app/project-data.json` and the class hooks in `app/styles.css`.
- `.vscode/launch.json` depends on the agreed app path and required URL, but not on the content of the dashboard; it can be created in parallel with the Phase 1 stylesheet and data.
- The Designer's stylesheet and the Coder's data and launch files have distinct ownership and can be worked on in parallel.
- Markup integration must follow the CSS/data contract handoffs. Final visual and runtime validation must follow all implementation work.
- Do not have the Designer and Coder edit the same file concurrently. Designer review is advisory; the file owner applies changes.

## Validation expectations

- Parse both JSON files (`app/project-data.json` and `.vscode/launch.json`) with a JSON parser; confirm the data has a top-level `projects` array and every record has all required fields plus `summary`.
- Open **Run Project Pulse Dashboard** in VS Code. Confirm the configured working directory is `${workspaceFolder}/app`, the local server serves that directory, and the browser opens `index.html` rather than a directory listing.
- Load the page through the configured server and verify that project records render with owner, status, recent activity, priority, and summary. Confirm the page handles empty or unavailable project data with a clear message and does not leave a failed or indefinite loading state.
- Check the rendered page at narrow and wide viewport sizes. Verify card layout, readable spacing, visible status/priority distinctions, semantic structure, keyboard focus visibility, and sufficient text contrast.
- Check the browser console and network requests for load/runtime errors, including the JSON request.

## Risks and open questions

- No application files or test harness exist yet. Keep the implementation static and avoid adding build tooling unless the launch environment requires it.
- The brief does not specify project names, status vocabulary, or priority levels. Use a small, representative sample and consistent labels; Mona can replace the sample data later.
- Ensure the selected local server is available in the Codespace and its launch output can reliably trigger opening `http://localhost:<port>/index.html`. If the proposed command is unavailable, choose an installed equivalent without changing the app's static-file contract.
