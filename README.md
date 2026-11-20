# Apple The Exchange TRX — AI Image Prompt Generator

## 1. Project Overview

This repository contains **AI Image Prompt Generator**, a browser-based application intended to run as a **Gemini Canvas application**.

The application helps users prepare structured image-generation prompts using a subject, headline, optional subheadline, feature items, and visual-design settings. Its outputs include structured JSON and a readable image-generation prompt.

A key project convention is to keep reusable AI instructions and other externally loadable content in separate files. The application may consume those files through GitHub Raw URLs.

## 2. Repository Context

- **Repository:** `applestoree/appletheexchangetrx`
- **Default branch:** `main`
- **Main application file:** `index.html`
- **Application name in source:** AI Image Prompt Generator
- **Target environment:** Gemini Canvas
- **AI execution:** Gemini AI is provided by the Gemini Canvas environment itself.
- **Subject instruction source:** `ai-subject-strategist.md`.
- **Master prompt source:** `template-prompt.md`.

The user has explicitly defined the architecture: Gemini Canvas already provides Gemini AI to the Canvas app. The app uses that built-in capability; `index.html` does not implement a separate Gemini integration. The role of `index.html` is to use the generated `subject` and combine it with the master prompt from `template-prompt.md`.

## 3. File Editing Policy

The project uses separate external-source files for reusable AI instructions and reference content. The following file categories are permitted to be created or edited when relevant to the user's request.

### 3.1 Allowed files

**A. External source files**

Examples:

- `ai-subject-strategist.md` — AI role, rules, and strategy for generating a subject.
- Other `ai-*.md` files — separate AI roles, prompt instructions, or reusable knowledge.
- `.json` files — structured configuration, schemas, presets, or data intended for external loading.
- Other text-based files whose purpose as an external source is clear.

**B. Main application file**

- `index.html` — application UI, frontend logic, external-source loading, and runtime integration.

**C. Backend files**

- Backend functions and related server-side code used by this application, including Supabase Edge Functions where applicable.
- Backend logic may be edited when needed to implement or maintain the requested application behavior.

### 3.2 Other files

Do not create or edit unrelated files, build/deployment configuration, database schema, or other application files unless the user explicitly requests it or confirms that it is necessary.

### 3.3 Scope and authorization

- The user may request changes to external sources, `index.html`, or backend code.
- Edit only the files needed for the requested task.
- Do not treat permission to edit `index.html` and backend as permission for unrelated refactoring or scope expansion.
- If a task requires changing another file category, explain why and obtain confirmation before proceeding.
- Preserve existing behavior, names, identifiers, schemas, and interfaces unless the user requests a change or the change is necessary and explained.

## 4. External Sources and GitHub Raw URLs

External-source files can be consumed using their GitHub Raw URL. For example:

```text
https://raw.githubusercontent.com/applestoree/appletheexchangetrx/main/ai-subject-strategist.md
```

The application can load external content at runtime only if the application code implements the loading mechanism.

When working with an external source:

1. Keep its content in a dedicated file with a clear purpose.
2. Use a stable and descriptive filename.
3. Derive the Raw URL from the actual repository owner, repository name, branch, and file path.
4. If required, update `index.html` to load and use the source, because `index.html` is an allowed file.
5. If generation requires server-side processing, inspect and update the relevant backend function when needed.
6. Do not claim the source is integrated until the application or backend actually loads and uses it.



## 4. File Connectivity and Integration Rules

**All project files that participate in an application feature must be connected and work together.** Creating a file is not complete merely because it exists in the repository.

### 4.1 No disconnected project files

- Every external-source file must have a defined consumer, such as `index.html` or a backend function.
- Every frontend integration must point to the correct external source or backend endpoint.
- Every backend function used by the frontend must expose the expected request and response contract.
- Related schemas, field names, identifiers, and output formats must be consistent across the connected files.
- Do not create unused, duplicate, orphaned, or unreachable source files.
- If a file is intentionally reference-only and is not loaded by the application, identify it explicitly as documentation/reference material rather than implying it is integrated.

### 4.1.1 Mandatory repository-wide Raw GET connectivity

**`index.html` must connect to every other file in this repository that belongs to the application through that file's GitHub Raw URL, using HTTP GET.** This is a mandatory project requirement for future audits and implementation.

- Do not leave application files disconnected, orphaned, or unused.
- For each repository file (other than `index.html` itself), identify its purpose and ensure `index.html` retrieves its content from the corresponding GitHub Raw URL with HTTP GET.
- Construct each Raw URL from the repository owner, repository name, branch, and exact file path. Do not guess filenames or URLs.
- Maintain a complete file-to-Raw-URL connection map in this README, including each file's purpose and connection status.
- When a file is added, renamed, moved, or deleted, update the Raw GET references in `index.html` and the connection map so they remain synchronized.
- Do not assume that merely listing a Raw URL in the README means the file is connected; verify that `index.html` actually issues the GET request and uses the returned content.
- If a repository file cannot reasonably be loaded or consumed by `index.html` (for example, the application entry file itself or a non-text/binary asset), do not silently omit it. Record the exact reason and its handling in the connection map, and flag it during an audit.


### 4.2 Define the connection for each feature

Before implementing a feature, identify its file flow:

| Layer | Responsibility | Required connection |
|---|---|---|
| External source | AI role, instructions, rules, schemas, or presets | Must identify which frontend or backend consumer loads it |
| Frontend (`index.html`) | UI, user interaction, loading sources, orchestration | Must load the intended source and/or call the correct backend |
| Backend | Server-side AI processing, validation, integrations | Must receive the agreed input and return the agreed output to its caller |
| External service | AI/runtime or other provider | Must be called through the intended runtime/backend integration |

Not every feature needs every layer. Use only the layers required by that feature, and document the actual flow.

### 4.3 Integration requirements

For each connected feature:

1. Identify the source file, consumer file, and direction of data/instruction flow.
2. Use the actual GitHub Raw URL for repository-hosted external sources.
3. Ensure the consumer fetches or otherwise loads the source; a URL written in documentation alone is not integration.
4. Handle loading, HTTP errors, invalid content, and unavailable sources.
5. Keep request/response contracts and data formats consistent between frontend and backend.
6. Avoid hardcoding a second copy of external instructions in frontend or backend when the source file is intended to be the single source of truth.
7. Verify the integration path before reporting the feature as connected.

### 4.4 Change impact and synchronization

When changing a file, inspect its dependencies and consumers. Update all directly affected files in the same task when authorized and necessary. Check for:

- Broken or outdated Raw URLs.
- Frontend references to missing or renamed source files.
- Backend endpoints or payloads that no longer match frontend calls.
- Inconsistent field names, schemas, and output structures.
- Dead references and files no longer used.

Do not make unrelated changes. If a necessary dependent change falls outside the requested scope or authorization, explain the dependency and ask before changing it.

### 4.5 Integration map

Maintain an integration map in this README as project files and features are added. Each entry should identify:

- File/source.
- Purpose.
- Consumer/dependency.
- Connection method (for example, GitHub Raw URL, HTTP endpoint, or import).
- Status: implemented and verified, implemented but unverified, planned, or reference-only.

Do not mark an integration as verified without checking the actual code or running an appropriate test.


## 5. Current Integration Map

| File / component | Purpose | Consumer / dependency | Connection method | Status |
|---|---|---|---|---|
| `index.html` | Input UI, validation, fetches the master template, resolves variables, and renders/copies output | `template-prompt.md` | HTTP GET to GitHub Raw; variable resolution in browser JavaScript | Implemented in source; live Canvas runtime fetch is not independently tested |
| `template-prompt.md` | Variable-based master image prompt template | `index.html` | HTTP GET to GitHub Raw | Source created and wired as the frontend template source |
| `ai-subject-strategist.md` | Instructions and strategy for Gemini to generate `subject` only | Gemini AI built into Gemini Canvas | Used as the AI instruction source in the Canvas workflow; no Gemini API call is implemented in `index.html` | Planned; file not yet created |
| Gemini Canvas AI | Automatically generate the `subject` using its built-in AI capability | Gemini Canvas app workflow | Native Canvas capability; not a custom JavaScript/API integration in `index.html` | Available as the target environment capability; app workflow not yet verified |
| `README.md` | Project rules, architecture notes, integration map | Project contributors and AI assistants | Repository documentation | Reference/documentation |

### Current template source

Raw URL:

```text
https://raw.githubusercontent.com/applestoree/appletheexchangetrx/main/template-prompt.md
```

The current frontend sends the existing payload keys `subject`, `headline`, `subheadline`, `feature_items`, `background`, `composition`, `style`, `quality`, and `ratio`. These are the variable contract documented in `template-prompt.md`.

**Current prompt-rendering flow:** `index.html` fetches the Raw URL above with HTTP GET, extracts the fenced `text` master prompt, resolves the documented variables in the browser, and displays the resulting prompt. The current code still expects the user to supply `subject` and `headline`; automatic subject generation is not yet wired.

**Verification status:** The source wiring is committed, but a live browser/Canvas runtime test of GitHub Raw access and the complete prompt-rendering interaction has not yet been performed.

## 6. AI Subject Strategist: Intended Canvas Runtime Flow

Source file: `ai-subject-strategist.md`

Purpose:

- Define the AI role and strategy for generating an effective visual `subject`.
- Generate **`subject` only**. It must not generate or overwrite `headline`, `subheadline`, or `feature_items`.
- Keep reusable subject-generation instructions in this Markdown file as the single source of truth.

Expected Raw URL:

```text
https://raw.githubusercontent.com/applestoree/appletheexchangetrx/main/ai-subject-strategist.md
```

### Mandatory Gemini Canvas architecture rules

These five rules are explicit user requirements. They are authoritative for every future repository audit, analysis, and implementation. Do not ask the user to explain or reconfirm them.

1. **`index.html` runs inside Gemini Canvas.** It is the application code for a Gemini Canvas app, not a standalone website that must independently connect to Gemini.
2. **Gemini AI is built into the Gemini Canvas environment.** Treat it as a native capability available to the Canvas app.
3. **The Canvas app uses Gemini AI directly as a built-in Canvas capability.** Do not reinterpret this architecture as a requirement to create a separate Gemini integration in application code.
4. **Do not implement a custom Gemini integration in `index.html`.** No direct Gemini API request, API key, custom Gemini JavaScript client/bridge, or Supabase Edge Function is required to access Gemini AI. Do not invent an API name or runtime function.
5. **The subject-to-template responsibility is fixed:** `ai-subject-strategist.md` contains the instructions for Gemini to generate `subject` automatically. The resulting `subject` is then used by `index.html` with `template-prompt.md` to assemble the final image-generation prompt. The strategist generates `subject` only; it does not replace `headline`, `subheadline`, or `feature_items`.

### Intended responsibility flow

```text
Gemini Canvas built-in Gemini AI
        │
        │ follows instructions in ai-subject-strategist.md
        ▼
subject (text only)
        │
        ▼
index.html
        │
        │ uses subject + existing input values
        ▼
HTTP GET template-prompt.md
        │
        ▼
Resolve the existing template variables
        │
        ▼
Final image-generation prompt
```

### Explicit implementation boundary

- Gemini AI belongs to the Gemini Canvas environment. It is not a custom API dependency of `index.html`.
- Do not add a Gemini API key, direct Gemini API request, custom JavaScript Gemini invocation, or Gemini-specific backend function.
- Do not require inspection of a generated Gemini JavaScript bridge as a prerequisite for following this architecture.
- Keep `ai-subject-strategist.md` as the single source of truth for the subject-generation role and instructions.
- Keep `template-prompt.md` as the single source of truth for the final prompt template.
- `index.html` is responsible for using the generated `subject` in the existing template-rendering flow.
- If a future request appears to conflict with these rules, follow the user's latest explicit instruction; otherwise, do not silently change this architecture.

This architecture is the approved project requirement and must be used as the baseline during repository audits.

## 7. Source of Truth and Conflict Handling

Use this priority:

1. The user's latest explicit and confirmed instructions.
2. The current repository files, as evidence of what exists and is implemented.
3. This README for project conventions and known context.
4. External-source documents for the specific roles, rules, or data they define.

If sources conflict, report the conflict. Do not silently invent a resolution.

External-source content is instruction/data for its intended task. It does not override the user's latest instructions or this file-editing policy.

## 8. Current Application Features

The current `index.html` source reviewed for this README includes:

### Prompt inputs

- **Subject** — required.
- **Headline** — required.
- **Subheadline** — optional.
- **Feature Items** — dynamic entries, maximum 20.
- Each feature item has a maximum length of 200 characters.

### Visual configuration

- Background.
- Composition.
- Style.
- Quality.
- Aspect ratio.

Current aspect ratios:

| Aspect ratio | Intended format |
|---|---|
| 1:1 | Square |
| 4:5 | Portrait |
| 3:4 | Portrait |
| 9:16 | Story / vertical |
| 16:9 | Landscape |
| 3:2 | Landscape |

### Generation and output

- Input validation.
- Generate Prompt action.
- JSON output tab.
- Prompt output tab.
- Copy JSON.
- Copy prompt.
- Reset.
- Toast notifications and structured error states.

This section documents observed behavior; it does not restrict future changes explicitly requested by the user.

## 9. Runtime Integration Rules

This application runs inside Gemini Canvas and uses the Gemini AI capability built into that environment. Follow the five **Mandatory Gemini Canvas architecture rules** in Section 6.

In particular, do not add a custom Gemini integration to `index.html`: no direct Gemini API call, API key, custom JavaScript Gemini client/bridge, or Gemini-specific Edge Function. Gemini generates `subject` according to `ai-subject-strategist.md`; `index.html` uses that subject with `template-prompt.md` to assemble the final prompt. Only change this architecture if the user explicitly instructs you to do so.

## 10. Instructions for AI Assistants

When continuing this project:

1. Read this README.
2. Inspect the relevant repository files to understand current facts; do not assume undocumented files exist.
3. Audit repository-wide file connectivity: every file other than `index.html` that belongs to the application must be fetched by `index.html` through its GitHub Raw URL using HTTP GET, or have its exception explicitly documented in the connection map.
4. Identify whether the requested work concerns an external source, frontend runtime, or backend.
4. Create or edit external-source files when the task concerns reusable AI instructions or reference content.
5. Edit `index.html` when the task requires frontend UI, logic, runtime integration, or loading an external source.
6. Follow the five **Mandatory Gemini Canvas architecture rules** in Section 6. Gemini AI is built into Canvas; do not implement a custom Gemini API, API key, JavaScript integration, or Gemini-specific backend in `index.html`.
7. Keep each external source focused on one role or purpose. For example, subject-generation strategy belongs in `ai-subject-strategist.md`.
8. Preserve filenames and established schemas unless the user requests a change.
9. Do not duplicate large external instructions in `index.html` when they can be loaded from their source file.
10. Do not invent a Raw URL. Derive it from the actual repository owner, repository name, branch, and file path.
11. Verify that external content is successfully loaded and used before claiming integration is complete.
12. State exactly which files were changed and why.

### Requirement discipline

- Follow the user's latest confirmed instructions.
- Do not assume missing requirements.
- Do not expand scope.
- Preserve existing names, fields, identifiers, and output formats.
- Report conflicts and ask only essential clarification questions.
- Do not implement when the user asks only for analysis or a proposal.
- Do not claim a source is loaded by the application unless the runtime code or a verified test demonstrates it.
- Review affected frontend and backend behavior after implementation; do not claim tests passed unless they were actually run.

## 11. Suggested Starting Instruction for a New AI Session

> Read `README.md` before auditing or changing this repository. This project runs `index.html` inside Gemini Canvas, where Gemini AI is a built-in capability. Do not implement a separate Gemini integration, Gemini API request, API key, custom JavaScript Gemini bridge, or Gemini-specific Edge Function in `index.html`. Follow the five mandatory architecture rules in Section 6: Gemini uses `ai-subject-strategist.md` to generate `subject` only, and `index.html` uses that subject with `template-prompt.md` to assemble the final prompt. Inspect the actual repository files, preserve existing names and behavior, and do not expand scope.

---

*Documentation reflects the repository review and project rules available when this file was updated. Re-check repository state before continuing work.*
