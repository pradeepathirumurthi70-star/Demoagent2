# Project Pulse dashboard implementation plan

## Summary

Build Mona's Project Pulse as a lightweight static dashboard for contributors. It
will show active projects, owners, status, recent activity, priority or risk, and
a concise contributor-friendly summary. The repository brief requires a
top-level `projects` array in the JSON data, with `name`, `owner`, `status`,
`recentActivity`, and `priority` on each project. Use plain HTML, CSS, and
JavaScript with no framework or build step; load the data from the local JSON
file and provide a VS Code launch configuration that serves the `app/` folder
and opens `index.html`.

## Responsibilities and file assignments

| Agent | Responsibility | Assigned files |
| --- | --- | --- |
| Planner | Research repository requirements and edge cases, define implementation phases, dependencies, and ownership. | `docs/project-pulse-plan.md` |
| Orchestrator | Coordinate the Planner, Designer, and Coder; establish shared markup/data contracts, manage sequencing, and review the integrated result. | No dashboard implementation files |
| Designer | Define the dashboard's information hierarchy and accessible, responsive visual treatment. Implement polished project cards, status badges, priority/risk treatment, clear spacing and typography, and deterministic `.dashboard` and `.project-card` styling hooks. | `app/styles.css` |
| Coder | Implement semantic dashboard markup and data loading/rendering; create representative project data; configure a deterministic VS Code launch target that serves the app directory and opens the dashboard. Handle loading, empty-data, and data-load error states. | `app/index.html`, `app/project-data.json`, `.vscode/launch.json` |

The Coder owns the markup and data contract; the Designer owns CSS only. The
Orchestrator must communicate the planned class hooks and content structure
before implementation so the separate file assignments integrate without
conflicts.

## Ordered implementation steps and dependencies

1. **Confirm shared contracts (Orchestrator).** Use this plan and
   `.github/project-pulse-brief.md` to agree on the dashboard structure,
   `.dashboard` and `.project-card` hooks, data fields, and status/priority
   presentation. Keep required fields intact; include a short summary field in
   the sample data if needed to supply the contributor-friendly summary.
2. **Implement the markup and data (Coder).** Create `app/index.html` with
   semantic page regions, responsive-friendly card markup, and accessible text.
   Load `app/project-data.json` and render the `projects` array. Give users
   clear loading, empty, and load-failure states; do not rely on color alone to
   communicate status or priority.
3. **Implement the visual design (Designer), in parallel with step 2 after
   step 1's contracts are agreed.** Create `app/styles.css` against the shared
   markup and hooks. Ensure the first view reads as a polished Project Pulse
   dashboard and that cards, badges, priority/risk, typography, spacing, and
   layout remain clear at narrow and wide viewport sizes.
4. **Create the runnable configuration (Coder, after step 2 establishes the
   entry point).** Add `.vscode/launch.json` as strict JSON with a
   **Run Project Pulse Dashboard** configuration. Serve from `${workspaceFolder}/app`
   and open `index.html` at a deterministic local URL, rather than opening a
   server directory listing.
5. **Integrate and validate (Orchestrator with Designer and Coder).** Resolve
   markup/style mismatches, verify the displayed fields and states, then run
   the checks below and report any remaining limitations.

### Parallel work decisions

- After step 1, the Designer can work on `app/styles.css` in parallel with the
  Coder working on `app/index.html` and `app/project-data.json`; they have
  disjoint file ownership and a shared class/data contract.
- The Coder's `.vscode/launch.json` work depends on confirming the entry point
  and run approach in step 2, so it follows the initial markup decision. It
  can proceed while the Designer finishes CSS once the app entry point is set.
- Final integration and browser/launch validation are sequential: they depend
  on all implementation files being present.

## Validation expectations

- Parse `app/project-data.json` and `.vscode/launch.json` as JSON. Confirm the
  data has a top-level `projects` array and every project has `name`, `owner`,
  `status`, `recentActivity`, and `priority`.
- Confirm the page references its local stylesheet and data correctly,
  provides `.dashboard` and `.project-card` hooks, and renders the required
  project information plus a short contributor-friendly summary.
- Review loading, empty-project, and JSON-load failure behavior. Verify status
  and priority remain understandable without color, and review keyboard
  navigation, semantic structure, text contrast, and responsive layouts.
- Start the **Run Project Pulse Dashboard** configuration in VS Code. Confirm
  it serves from `app/`, opens the dashboard `index.html`, loads project data,
  and does not display a directory listing. Check browser console/network
  output for failed assets or data requests.
- Run `bash scripts/validate-exercise.sh` to check repository exercise
  invariants after implementation; supplement it with the app and launch
  checks above because the script does not run the dashboard.

## Edge cases and assumptions

- The brief requires the listed project fields but does not prescribe sample
  records or allowed status/priority values. Keep sample values consistent and
  use readable labels; do not make the UI depend on one exact set of values.
- Handle an empty `projects` array distinctly from a failed JSON request.
- Keep asset and data URLs relative to the app so they work when served from
  the configured `app/` working directory.
- No repository app framework or package manifest currently exists, so use
  browser-native technologies and avoid introducing a dependency unless
  implementation research reveals a concrete need.
