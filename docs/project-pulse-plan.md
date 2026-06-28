# Project Pulse implementation plan

## Summary
Build a lightweight static **Project Pulse** dashboard in `app/` using vanilla HTML, CSS, and inline JavaScript in `app/index.html` to load `app/project-data.json` and render project cards. Add `.vscode/launch.json` so VS Code can run **Run Project Pulse Dashboard** with `python3 -m http.server 5500` from the `app/` directory and open `index.html` directly.

## Designer responsibilities
- Define the dashboard information hierarchy and first-view layout.
- Make the UI polished, readable, and responsive.
- Specify and/or implement the visual treatment for project cards, status badges, and priority emphasis.
- Ensure accessibility basics: semantic structure support, contrast, spacing, focus on readability, and mobile-safe layout.
- Keep CSS hooks deterministic, especially `.dashboard` and `.project-card`.

## Coder responsibilities
- Implement deterministic static behavior within assigned files.
- Create valid `app/project-data.json` with the required top-level `projects` array.
- Create strict JSON in `.vscode/launch.json` with no comments.
- Build `app/index.html` so it references `styles.css` and `project-data.json`, renders visible project cards, and shows `name`, `owner`, `status`, `recentActivity`, and `priority`.
- Validate JSON parsing and launch behavior before handoff.

## Ordered implementation steps

### 1. Lock scope and implementation contract
**Goal:** Confirm the static-app approach and shared contracts before file work starts.

**File assignments**
- No production file edits required in this step.
- Contract to carry forward:
  - `app/index.html` must contain the visible dashboard and inline logic.
  - `app/styles.css` must contain polished dashboard styling.
  - `app/project-data.json` must use a top-level `projects` key.
  - `.vscode/launch.json` must serve from `app/` and open `index.html`.

**Notes**
- Use no framework and no build tooling to match the repository's current minimal pattern.
- Plan for `fetch('project-data.json')` from `app/index.html`; this requires a local server, which is why the launch config matters.

---

### 2. Create the design contract
**Goal:** Define the visual structure before HTML and data rendering are finalized.

**File assignments**
- **Designer primary:** `app/styles.css`
- **Designer consulted:** `app/index.html`

**Expected output**
- Layout direction for the page header, project grid/list, card anatomy, and badge treatment.
- Required HTML hooks/classes, including `.dashboard` and `.project-card`.
- Responsive behavior rules and accessibility guidance.

**Dependency**
- This step should finish before `app/index.html` is finalized, because HTML structure and class names must match the CSS contract.

---

### 3. Build the data and run configuration foundation
**Goal:** Establish the content model and runnable preview support.

**File assignments**
- **Coder primary:** `app/project-data.json`
- **Coder primary:** `.vscode/launch.json`

**Expected output**
- `app/project-data.json` with a top-level `projects` array.
- Each project includes at least:
  - `name`
  - `owner`
  - `status`
  - `recentActivity`
  - `priority`
- `.vscode/launch.json`:
  - strict JSON with no comments
  - launch name **Run Project Pulse Dashboard**
  - command `python3 -m http.server 5500`
  - `cwd` pointing to `${workspaceFolder}/app`
  - `serverReadyAction` opening `http://localhost:%s/index.html`

**Dependencies**
- Independent from final HTML markup details.
- `app/index.html` depends on the final JSON key names from this step.

---

### 4. Implement the dashboard UI
**Goal:** Render the actual Project Pulse dashboard frontend.

**File assignments**
- **Coder primary:** `app/index.html`
- **Designer primary:** `app/styles.css`
- **Designer consulted:** `app/index.html`
- **Coder consulted:** `app/styles.css` only if integration reveals selector mismatches

**Expected output**
- `app/index.html`:
  - exact title `Project Pulse`
  - references `styles.css`
  - references `project-data.json`
  - uses inline JavaScript to fetch/load data
  - renders visible project cards with the `project-card` class
  - shows `status`, `recentActivity`, and `priority` in the UI
- `app/styles.css`:
  - includes `.dashboard`
  - includes `.project-card`
  - includes `border-radius`
  - includes `box-shadow`
  - delivers polished spacing, hierarchy, and responsive behavior

**Dependencies**
- Depends on Step 2 for CSS hooks and layout direction.
- Depends on Step 3 for the final data shape in `app/project-data.json`.

---

### 5. Validate and integrate
**Goal:** Confirm the dashboard works as a complete exercise deliverable.

**File assignments**
- Validation touches:
  - `app/index.html`
  - `app/styles.css`
  - `app/project-data.json`
  - `.vscode/launch.json`

**Expected output**
- All files exist and parse where required.
- Launching **Run Project Pulse Dashboard** opens the dashboard page, not a directory listing.
- The rendered page visibly looks like a polished dashboard.

## Dependencies between steps
- **Step 2 → Step 4:** HTML structure should align with the Designer's selector and layout contract.
- **Step 3 → Step 4:** `app/index.html` must use the exact JSON keys created in `app/project-data.json`.
- **Step 4 → Step 5:** Validation is meaningful only after UI, data, and launch config exist.

## Dependencies between file assignments
- `app/index.html` depends on:
  - `app/project-data.json` for field names and content
  - `app/styles.css` for matching selectors/classes
- `app/styles.css` depends on:
  - stable class names in `app/index.html`
- `.vscode/launch.json` depends on:
  - the app living in `app/`
  - `index.html` being the intended entry page

## Parallel work decisions

### Can run in parallel
- **Designer on `app/styles.css`** and **Coder on `app/project-data.json` + `.vscode/launch.json`**
  - **Rationale:** no overlapping files once the shared contract is clear.
- Validation of JSON structure for `app/project-data.json` and `.vscode/launch.json`
  - **Rationale:** independent of final CSS polish.

### Must remain sequential
- Final `app/index.html` implementation after the design contract and JSON keys are stable
  - **Rationale:** it depends on both selector names and data shape.
- Final CSS cleanup after HTML integration, if needed
  - **Rationale:** avoids Designer/Coder conflicts over selectors and markup assumptions.
- Full launch validation after all files exist
  - **Rationale:** the browser target only works when `index.html` and supporting files are present.

## Edge cases to handle
- `project-data.json` fails to load because the page is opened without a server.
- Empty `projects` array.
- Missing or unexpected project fields in one record.
- JSON parse error.
- Launch config opens the app root and shows a directory listing instead of `index.html`.
- Status or priority values vary in wording and need graceful display.
- Small-screen layouts where cards stack and remain readable.

## Validation expectations
- `app/index.html`, `app/styles.css`, `app/project-data.json`, and `.vscode/launch.json` exist.
- `app/project-data.json` parses as valid JSON.
- `.vscode/launch.json` parses as valid JSON.
- `app/index.html` includes:
  - `Project Pulse`
  - `styles.css`
  - `project-data.json`
  - `project-card`
  - `status`
  - `recentActivity`
  - `priority`
- `app/styles.css` includes:
  - `.dashboard`
  - `.project-card`
  - `border-radius`
  - `box-shadow`
- `.vscode/launch.json` includes:
  - `Run Project Pulse Dashboard`
  - `index.html`
  - serving from `app/`
  - `http://localhost:%s/index.html`
- Manual run in VS Code confirms the browser opens the dashboard frontend, not a directory listing.
- Manual review confirms the page is polished, readable, and responsive.

## Open questions
- Should the "contributor-friendly summary" be page-level copy, or should each project also get an optional summary field?
- How many seed projects should be included? Recommendation: 3-5 so the dashboard looks complete without becoming crowded.
- Should status and priority values use a fixed vocabulary for consistent badge styling? Recommendation: yes.
