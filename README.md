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
- **Intended AI runtime:** Gemini Canvas runtime without requiring the user to provide an API key.

The `index.html` source reviewed for this documentation sends generation requests to a Supabase Edge Function. It does not directly invoke a Gemini runtime API in the browser source. Do not describe native Gemini Canvas runtime integration as implemented unless the current source proves it.

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

## 5. Example External Source: AI Subject Strategist

Example file: `ai-subject-strategist.md`

Purpose:

- Define an AI role responsible for creating an effective visual subject.
- Explain the relationship between category, subcategory, content pillar, and content type when those inputs are supplied.
- Define how to derive a clear subject from available context.
- Define behavior for missing or incomplete input.
- Specify output format and quality criteria.

Expected Raw URL:

```text
https://raw.githubusercontent.com/applestoree/appletheexchangetrx/main/ai-subject-strategist.md
```

This is an example of an external source file and URL structure, not a claim that the file has already been created or integrated. Create the file only when requested. Keep reusable strategy in the Markdown source and let the application load it, rather than duplicating the entire strategy in frontend code.

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

## 8. Current Generation Integration (Observed)

The current frontend defines this endpoint:

```text
https://axednjikcambzbkxyqpp.supabase.co/functions/v1/generate-image-prompt
```

The `handleGeneratePrompt()` flow uses native `fetch()` to send a POST request with JSON content. The frontend expects:

- `success === true`
- A populated `data` field

The reviewed frontend does not contain a direct Gemini API call or a Gemini API key. This file alone does not establish whether the Supabase Edge Function calls Gemini internally; that requires inspecting the function implementation.

The intended target remains Gemini Canvas runtime without a user-supplied API key. Do not add an API-key input or hardcode secrets in frontend code. Keep credentials and secrets in appropriate secure backend configuration.

## 9. Instructions for AI Assistants

When continuing this project:

1. Read this README.
2. Inspect the relevant repository files to understand current facts; do not assume undocumented files exist.
3. Identify whether the requested work concerns an external source, frontend runtime, or backend.
4. Create or edit external-source files when the task concerns reusable AI instructions or reference content.
5. Edit `index.html` when the task requires frontend UI, logic, runtime integration, or loading an external source.
6. Edit the relevant backend function when the task requires server-side generation, API integration, validation, or backend behavior.
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

## 10. Suggested Starting Instruction for a New AI Session

> Read `README.md` and inspect the relevant repository files before working. This project supports standalone external-source files, such as `ai-subject-strategist.md`, which may be consumed through GitHub Raw URLs. You may create or edit external-source files, `index.html`, and relevant backend files when required by the user's task. Do not modify unrelated files or expand scope without confirmation. Keep reusable AI instructions in dedicated external-source files, and update the frontend/backend integration when necessary to load and use them. Follow the user's latest confirmed requirements and verify behavior before reporting completion.

---

*Documentation reflects the repository review and project rules available when this file was updated. Re-check repository state before continuing work.*
