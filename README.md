# Apple The Exchange TRX — AI Image Prompt Generator

## 1. Project Overview

This repository contains the source code for **AI Image Prompt Generator**, a browser-based application intended to run as a **Gemini Canvas application**.

The application helps users prepare structured image-generation prompts by entering a subject, headline, optional subheadline, feature items, and visual-design settings. It then generates two outputs:

- A structured JSON representation.
- A readable image-generation prompt.

The primary project goal is to let an AI assistant understand the application quickly and continue work without losing the existing product behavior, code structure, or user decisions.

## 2. Important Project Context

- **Repository:** `applestoree/appletheexchangetrx`
- **Main application file:** `index.html`
- **Application name in the source:** AI Image Prompt Generator
- **Target environment:** Gemini Canvas
- **Intended AI runtime:** Gemini Canvas's available Gemini runtime, without requiring the user to provide an API key.
- **Current implementation status:** The checked-in frontend currently sends generation requests to a Supabase Edge Function. It does not directly invoke a Gemini runtime API in the browser source.

> **Important distinction:** Gemini Canvas is the intended target environment, but the current `index.html` implementation must not be described as already using the native Gemini Canvas runtime. The actual request flow in the source currently points to the Supabase Edge Function documented below. Verify the runtime behavior and available Canvas APIs before changing this integration.

## 3. Source of Truth

Use these sources in this order:

1. The user's latest explicit instructions and confirmed requirements.
2. The current source code in this repository, especially `index.html`, for what is actually implemented.
3. This README for project context, intended direction, known integration details, and continuation rules.

If this README and the source code disagree, report the discrepancy instead of silently assuming either is correct. If a new user instruction changes a requirement, follow the latest confirmed instruction and update this README when asked or when the documentation update is part of the task.

## 4. Current Application Features

The current `index.html` includes the following user-facing capabilities:

### Prompt inputs

- **Subject** — required.
- **Headline** — required.
- **Subheadline** — optional.
- **Feature Items** — dynamic feature entries, with a maximum of 20 items.
- Each feature item has a maximum length of 200 characters.

### Visual configuration

The interface provides configuration for:

- Background.
- Composition.
- Style.
- Quality.
- Aspect ratio.

Supported aspect ratios in the current source:

| Aspect ratio | Intended format |
|---|---|
| 1:1 | Square |
| 4:5 | Portrait |
| 3:4 | Portrait |
| 9:16 | Story / vertical |
| 16:9 | Landscape |
| 3:2 | Landscape |

### Generation and output

- Input validation before generation.
- Generate Prompt action.
- JSON output tab.
- Prompt output tab.
- JSON copy action.
- Prompt copy action.
- Reset action.
- Toast notifications and structured error states.

Treat this list as a description of the current source, not a restriction against future changes requested by the user.

## 5. Current Generation Integration

The current frontend defines this endpoint:

```text
https://axednjikcambzbkxyqpp.supabase.co/functions/v1/generate-image-prompt
```

The `handleGeneratePrompt()` flow uses native `fetch()` to send a POST request with JSON content. The frontend expects a successful response with:

- `success === true`
- A populated `data` field

The frontend source does not contain a direct Gemini API call and does not contain a Gemini API key. It also does not establish that the Supabase Edge Function uses Gemini internally; that would require inspecting the Edge Function implementation.

### Integration decision to preserve

The intended target is Gemini Canvas runtime **without a user-supplied API key**. Before modifying the integration:

1. Inspect the current request and response contract in `index.html`.
2. Verify which Gemini runtime/API capabilities are actually available in the target Gemini Canvas environment.
3. Determine whether the Edge Function should be replaced, retained, or adapted.
4. Preserve the existing input/output behavior unless the user explicitly changes it.
5. Do not invent runtime APIs, global objects, or undocumented methods.

Do not add an API-key input or hardcode a secret into frontend code.

## 6. Frontend Dependencies Observed

The current HTML source loads frontend resources from CDNs, including:

- Tailwind CSS CDN.
- Google Fonts.
- Lucide icon library CDN.

Verify the current file before changing dependencies. Do not assume a build system, package manager, framework, or separate source directory exists unless it is present in the repository.

## 7. AI Assistant Instructions for Continuing This Project

When working on this repository, act as a careful code analyst and implementation assistant.

### Before making changes

1. Read this README.
2. Read the latest version of `index.html` from the repository's default branch.
3. Inspect the relevant functions and surrounding code before proposing a change.
4. Compare the user's latest request with the current implementation and the existing project requirements.
5. Identify missing information, conflicts, risks, and affected features.

### Requirement handling

- Treat the user's latest explicit and confirmed instructions as authoritative.
- Do not make assumptions where confirmation is needed.
- Do not expand the scope beyond the requested task.
- Preserve existing names, IDs, field names, function names, and behavior unless a change is requested or technically necessary and explained.
- If requirements conflict, describe the conflict and ask for confirmation before proceeding.
- Ask only essential clarification questions. If the requirements are sufficient, proceed without unnecessary questions.
- Do not begin implementation when the user is asking only for analysis, an audit, or a proposal.
- When implementation is requested, make the smallest safe change that satisfies the confirmed requirement.

### Code-change safety

- Preserve existing working features unless the user asks to remove or replace them.
- Avoid unrelated refactoring and dependency changes.
- Do not introduce mock data as a substitute for real application data or runtime behavior.
- Do not claim that a feature works without checking the relevant implementation.
- Do not expose API keys, tokens, credentials, or other secrets in client-side code.
- After changes, review the modified code for syntax errors, broken event handlers, invalid references, and regressions to adjacent functionality.
- Report exactly which files were changed and summarize the behavior changed.
- If a requested capability depends on a backend or runtime that is not available for inspection, state that limitation clearly.

## 8. Current Known Gap

The main known architectural gap is the difference between the intended runtime and the current frontend integration:

| Area | Intended direction | Current source observation |
|---|---|---|
| Execution environment | Gemini Canvas | User-defined target environment |
| AI generation runtime | Gemini Canvas runtime | Frontend calls a Supabase Edge Function |
| API key | No user-provided API key | No Gemini API key in `index.html` |
| Generation endpoint | To be verified against Canvas runtime capabilities | Supabase Edge Function URL is hardcoded in the frontend |
| Prompt UI and outputs | Preserve existing experience | Implemented in `index.html` |

This is a documented gap, not authorization to change the integration automatically. Resolve it according to the user's next confirmed instruction.

## 9. Suggested Starting Point for a New AI Session

Use this instruction when continuing the project:

> Read `README.md` and `index.html` in this repository before doing any work. Understand the current AI Image Prompt Generator, its existing UI and behavior, and its current Supabase Edge Function request flow. The intended deployment environment is Gemini Canvas and the intended Gemini runtime must work without a user-supplied API key. Do not assume the current source already uses that runtime. First analyze the requested task against the actual code, identify relevant gaps or conflicts, and preserve all existing functionality. Do not implement changes until implementation is explicitly requested and the requirements are sufficiently clear.

---

*Documentation status: created from the repository source review and project context available at the time of writing. Re-check the source when continuing development because the code may change independently of this document.*
