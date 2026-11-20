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

The current `index.html` source reviewed for this documentation sends generation requests to a Supabase Edge Function. It does not directly invoke a Gemini runtime API in the browser source. Do not describe the native Gemini Canvas runtime integration as implemented unless the current source proves it.

## 3. Critical File Editing Rule

<mark>Only files intended to be consumed as external sources may be created or edited.</mark>

This is a strict project boundary for AI assistants and contributors.

### Allowed changes

Create or update standalone external-source files whose contents can be loaded by the application or used as external reference material, for example:

- `ai-subject-strategist.md` — AI role, rules, and strategy for generating a subject.
- Other `ai-*.md` files — separate AI roles, prompt instructions, or reusable knowledge.
- `.json` files — structured configuration, schemas, presets, or data intended for external loading.
- Other text-based source files, only when their purpose as an external source is clear and the user requests them.

An external source should have a clear purpose, stable filename, and content format suitable for the intended consumer.

### Files that must not be edited under this rule

- `index.html` — application runtime/source code.
- Existing application code, UI, styles, scripts, and runtime logic.
- Build, deployment, or project configuration files.
- Backend functions or database code.

Do not modify these files as part of an external-source task. Do not refactor, reformat, or make incidental fixes to them.

### Important: Raw URL integration

External-source files can be consumed using their GitHub Raw URL. For example:

```text
https://raw.githubusercontent.com/applestoree/appletheexchangetrx/main/ai-subject-strategist.md
```

The application can load the external content at runtime if its existing implementation supports that mechanism.

**Do not edit `index.html` to add a Raw URL or wire up a new loader under the external-source-only rule.** If the requested external source cannot be consumed without changing application code, report that as an integration gap and ask the user for explicit authorization to change the application code. Never silently cross this boundary.

## 4. Example External Source: AI Subject Strategist

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

This is an example of an external source file and URL structure, not a claim that the file has already been created or integrated. Create the file only when requested. Keep its instructions in the Markdown file rather than embedding the full strategy into `index.html`.

## 5. Source of Truth and Conflict Handling

Use this priority:

1. The user's latest explicit and confirmed instructions.
2. The current repository files, as evidence of what exists and is implemented.
3. This README for project conventions and known context.
4. External-source documents for the specific roles, rules, or data they define.

If sources conflict, report the conflict. Do not silently invent a resolution or treat an external instruction file as permission to edit application code.

External-source content is instruction/data for its intended task. It does not override the user's latest instructions or the file-editing boundary in this README.

## 6. Current Application Features

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

This section documents observed behavior; it does not authorize edits to `index.html`.

## 7. Current Generation Integration (Observed)

The current frontend defines this endpoint:

```text
https://axednjikcambzbkxyqpp.supabase.co/functions/v1/generate-image-prompt
```

The `handleGeneratePrompt()` flow uses native `fetch()` to send a POST request with JSON content. The frontend expects:

- `success === true`
- A populated `data` field

The reviewed frontend does not contain a direct Gemini API call or a Gemini API key. This file alone does not establish whether the Supabase Edge Function calls Gemini internally; that requires inspecting the function implementation.

The intended target remains Gemini Canvas runtime without a user-supplied API key. Do not add an API-key input or hardcode secrets in frontend code.

## 8. Instructions for AI Assistants

When continuing this project:

1. Read this README.
2. Inspect the relevant repository files to understand current facts; do not assume undocumented files exist.
3. Identify whether the requested work concerns an external source or application runtime.
4. For external-source work, create or edit only the specific external-source file requested.
5. Keep each source focused on one role or purpose. For example, subject-generation strategy belongs in `ai-subject-strategist.md`.
6. Preserve filenames and established schemas unless the user requests a change.
7. Do not copy the entire contents of external Markdown instructions into `index.html`.
8. Do not edit `index.html`, application code, backend code, or configuration under the external-source-only rule.
9. If an external source needs application-code changes to become usable, explain the gap and request explicit authorization before making those changes.
10. Do not invent a Raw URL. Derive it from the actual repository owner, repository name, branch, and file path.
11. After creating or updating a source file, provide its GitHub file URL and Raw URL.
12. State exactly which external-source files were changed.

### Requirement discipline

- Follow the user's latest confirmed instructions.
- Do not assume missing requirements.
- Do not expand scope.
- Preserve existing names, fields, identifiers, and output formats.
- Report conflicts and ask only essential clarification questions.
- Do not implement when the user asks only for analysis or a proposal.
- Do not claim a source is loaded by the application unless the runtime code or a verified test demonstrates it.

## 9. Suggested Starting Instruction for a New AI Session

> Read `README.md` and inspect the relevant repository files before working. This project uses standalone external-source files, such as `ai-subject-strategist.md`, which may be consumed through GitHub Raw URLs. Only create or edit files intended to serve as external sources. Do not modify `index.html`, application runtime code, backend code, or configuration under this rule. If a task requires application-code changes to load an external source, explain the integration gap and request explicit authorization first. Follow the user's latest confirmed requirements and do not assume that a Raw URL is already integrated.

---

*Documentation reflects the repository review and project rules available when this file was updated. Re-check repository state before continuing work.*
