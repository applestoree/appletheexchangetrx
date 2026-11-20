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
- **Intended AI runtime:** Gemini Canvas app runtime, using the Gemini-powered capability added through Canvas's **Add Gemini features** workflow; no user-provided API key.
- **Runtime assumption approved by the user:** after Gemini features are added to this Canvas app, the generated app JavaScript can call the Canvas-provided Gemini capability. The app will use that capability to generate `subject` from the instructions loaded from `ai-subject-strategist.md`.

The current `index.html` fetches `template-prompt.md` from GitHub Raw and renders the master prompt in browser JavaScript. Gemini Canvas is the target execution environment. The project requirement is to use Gemini Canvas's **Add Gemini features** capability so the Canvas app can invoke Gemini-powered text generation from its own JavaScript runtime, without asking the user for a separate API key. The exact runtime bridge/function signature must be taken from the code that Gemini Canvas generates when this feature is added; do not invent an API name or claim the integration is implemented until that generated code is inspected.

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
| `ai-subject-strategist.md` | AI role and rules to generate `subject` only | `index.html` + Gemini Canvas AI capability | Planned: HTTP GET to GitHub Raw, then pass loaded instructions and product context to the Canvas-provided Gemini text-generation capability | Planned; file and integration not yet implemented |
| Gemini Canvas AI capability | Generate text (`subject`) within the Canvas app | `index.html` | Canvas **Add Gemini features** workflow; use the actual generated runtime call/bridge | Capability documented by Google; exact generated app code/signature not yet inspected |
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

Intended data flow:

```text
User product/context inputs
        │
        ▼
index.html — HTTP GET ai-subject-strategist.md
        │
        ▼
Gemini Canvas-provided text-generation capability
        │  instructions + user context
        ▼
subject (text only)
        │
        ▼
index.html — HTTP GET template-prompt.md
        │
        ▼
Resolve {{subject}} and other existing template variables
        │
        ▼
Final image-generation prompt
```

### Gemini Canvas runtime assumption and boundary

The user explicitly approves the implementation assumption that the Gemini Canvas app runtime exposes a Gemini-powered capability callable from the JavaScript of the Canvas app after the app's **Add Gemini features** capability has been enabled. `index.html` is intended to run as the app code inside Gemini Canvas, not as a separately hosted generic website.

Google's official Gemini Apps Help documents that users can add Gemini-powered features, including text and image generation, to a Canvas app through **Add Gemini features**. It does not document a stable public JavaScript API name or function signature in that help article. Therefore:

- Treat Canvas-provided Gemini text generation as the intended runtime mechanism, not an external Gemini API key or a new Supabase Edge Function.
- When implementing, enable/add Gemini features to this Canvas app and inspect the actual generated JavaScript/runtime bridge that Canvas inserts.
- Call the actual Canvas-generated mechanism from `index.html`; do not invent names such as `gemini.generateContent()` or assume a generic global exists without evidence in the generated app code.
- The Canvas AI call must receive the fetched strategist instructions and the relevant user inputs, and return a plain-text `subject` for the existing prompt pipeline.
- If Canvas has not yet injected/provided its AI runtime capability, automatic subject generation is unavailable until the feature is added. Do not silently fall back to a manual prompt or claim generation succeeded.
- Never place a private API key in client-side source.

Official reference: [Google Gemini Apps Help — Create docs, apps & more with Canvas](https://support.google.com/gemini/answer/16047321?hl=en), section **Add AI to your app**. The guide confirms Canvas can add Gemini-powered text/image generation, but the exact generated code must be inspected in the target app.

This flow is **planned**, not yet implemented. The strategist file must be created and the actual Canvas-generated runtime integration verified before this map entry can be marked implemented.

## 6. Source of Truth and Conflict Handling

Use this priority:

1. The user's latest explicit and confirmed instructions.
2. The current repository files, as evidence of what exists and is implemented.
3. This README for project conventions and known context.
4. External-source documents for the specific roles, rules, or data they define.

If sources conflict, report the conflict. Do not silently invent a resolution.

External-source content is instruction/data for its intended task. It does not override the user's latest instructions or this file-editing policy.

## 7. Current Application Features

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

The app is intended to run as code inside Gemini Canvas. Its automatic subject-generation path must use the Gemini-powered runtime capability added through Canvas **Add Gemini features**, based on the user's approved assumption that this capability is callable by the app's JavaScript.

Do not replace this with a direct Google Gemini API request that requires a user-managed API key, or with the previous `generate-image-prompt` Supabase Edge Function, unless the user explicitly changes the architecture. The existing `template-prompt.md` loading and variable-resolution flow remains a separate browser-side step.

The actual JavaScript invocation is implementation-dependent: inspect the code generated by Canvas after enabling **Add Gemini features**, then use that exact callable interface. The official help article confirms that Canvas can add Gemini-powered text and image generation, but does not specify the runtime API signature. Keep the implementation aligned with the generated Canvas code and verify it in the Canvas preview.

## 10. Instructions for AI Assistants

When continuing this project:

1. Read this README.
2. Inspect the relevant repository files to understand current facts; do not assume undocumented files exist.
3. Identify whether the requested work concerns an external source, frontend runtime, or backend.
4. Create or edit external-source files when the task concerns reusable AI instructions or reference content.
5. Edit `index.html` when the task requires frontend UI, logic, runtime integration, or loading an external source.
6. For subject generation, use the Gemini Canvas-provided text-generation capability added through **Add Gemini features**; inspect and reuse the actual generated runtime call rather than inventing an API signature. Do not add a backend or API key unless the user changes the architecture.
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

> Read `README.md` and inspect the relevant repository files before working. This project runs `index.html` inside Gemini Canvas. The approved design uses Canvas **Add Gemini features** for Gemini-powered text generation, with the Canvas-generated runtime call inspected and reused rather than an invented API signature or user-provided API key. `index.html` loads reusable AI instructions such as `ai-subject-strategist.md` and the master prompt `template-prompt.md` from GitHub Raw. The strategist generates `subject` only. You may edit the relevant external sources and `index.html`; do not modify unrelated files or expand scope. Verify the Canvas runtime integration and end-to-end behavior before reporting completion.

---

*Documentation reflects the repository review and project rules available when this file was updated. Re-check repository state before continuing work.*
