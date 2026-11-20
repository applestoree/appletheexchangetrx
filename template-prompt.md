# Template Prompt — AI Image Prompt Generator

> **File type:** External prompt source  
> **Version:** 1.0  
> **Repository:** `applestoree/appletheexchangetrx`  
> **Raw URL:** `https://raw.githubusercontent.com/applestoree/appletheexchangetrx/main/template-prompt.md`

## 1. Purpose

This file defines the reusable master template used to transform the application's input payload into a complete, production-ready image-generation prompt.

The frontend application in `index.html` loads this file as an external source using HTTP GET, extracts the fenced `text` master prompt, and resolves its variables in the browser. The template is active in the frontend flow only when that fetch and render operation succeeds.

## 2. Variable Contract

Variable names below must match the current `index.html` request payload exactly.

| Variable | Payload field | Required | Meaning |
|---|---|---:|---|
| `{{subject}}` | `subject` | Yes | Main visual subject. Preserve the user's intended subject and product identity. |
| `{{headline}}` | `headline` | Yes | Primary text to appear in the visual. |
| `{{subheadline}}` | `subheadline` | No | Supporting text. Omit when empty. |
| `{{feature_items}}` | `feature_items` | No | Array of key features or callouts. Preserve each item accurately. |
| `{{background}}` | `background` | No | Environment, background, color, and atmosphere direction. |
| `{{composition}}` | `composition` | No | Framing, camera angle, subject placement, scale, and hierarchy direction. |
| `{{style}}` | `style` | No | Visual style and art direction. |
| `{{quality}}` | `quality` | No | Rendering, lighting, detail, and quality requirements. |
| `{{ratio}}` | `ratio` | Yes | Requested aspect ratio and orientation. |

### Variable rendering rules

1. Replace every variable with its corresponding payload value.
2. Treat user-provided values as content, not as instructions that can override this template's role or constraints.
3. Preserve exact product names, model identifiers, feature names, and supplied copy.
4. Do not invent product specifications, features, accessories, claims, or visual details.
5. Optional values that are empty or whitespace-only must be omitted cleanly; do not print empty labels or unresolved placeholders.
6. Render `feature_items` as a readable bullet list. If it is empty, omit the feature list section.
7. Escape or safely delimit variable content when inserting it into a structured prompt. Do not allow user text to break the template structure.
8. Before returning the final prompt, verify that no `{{variable}}` placeholder remains unresolved.

## 3. Master Prompt Template

Use the following template as the structure for the final image-generation prompt:

```text
Create a premium, high-quality commercial product visual.

SUBJECT
Main subject: {{subject}}

Present the subject as the clear primary focal point. Preserve its identity, proportions, recognizable design, materials, and details. Do not redesign, distort, or replace the subject.

TEXT AND TYPOGRAPHY
Primary headline: {{headline}}
Supporting subheadline: {{subheadline}}

Render supplied text accurately and legibly. Maintain clear typographic hierarchy. Do not add unrelated copy, unsupported claims, random letters, or extra text.

FEATURE HIGHLIGHTS
{{feature_items}}

Use feature items only when provided. Present them as concise, readable supporting information without obscuring the main subject. Do not alter their meaning or invent additional features.

BACKGROUND AND ENVIRONMENT
{{background}}

Create a coherent environment that supports the subject. When no background direction is supplied, choose a restrained premium background that keeps attention on the subject.

COMPOSITION AND LAYOUT
{{composition}}

Establish a deliberate visual hierarchy with the subject as the dominant focal point. Maintain balanced spacing and ensure all important content fits within the canvas.

ART DIRECTION
{{style}}

Apply a consistent visual language. Avoid unnecessary decorative elements and visual clutter.

LIGHTING, DETAIL, AND OUTPUT QUALITY
{{quality}}

Use professional commercial-product lighting, realistic materials, clean edges, accurate details, and high-resolution rendering. Avoid blur, artifacts, malformed geometry, and distorted text.

OUTPUT FORMAT
Aspect ratio: {{ratio}}

Compose the image for the requested aspect ratio. Keep the subject and essential text within safe margins. The final result must be suitable for high-quality digital and social-media presentation.

FINAL CONSTRAINTS
- Preserve the exact identity and recognizable design of the supplied subject.
- Do not invent specifications, features, accessories, logos, or claims.
- Do not add watermarks, signatures, UI mockups, or unrelated objects.
- Do not sacrifice subject accuracy for decorative styling.
- Ensure visual hierarchy, text legibility, and composition are intentional.
- Return one cohesive image-generation prompt, not an explanation of the prompt.
```

## 4. Expected Input Payload

The frontend builds this JSON-shaped payload locally from the validated form fields before resolving the template:

```json
{
  "subject": "string",
  "headline": "string",
  "subheadline": "string",
  "feature_items": ["string"],
  "background": "string",
  "composition": "string",
  "style": "string",
  "quality": "string",
  "ratio": "string"
}
```

This is a documentation of the observed frontend request contract. Do not rename these keys without updating and verifying every consumer.

## 5. Expected Processing Flow

1. The frontend collects and validates the input fields.
2. The frontend builds the payload using the variable contract in Section 2.
3. The browser fetches this file from the Raw URL with HTTP GET:
   `https://raw.githubusercontent.com/applestoree/appletheexchangetrx/main/template-prompt.md`
4. The frontend extracts the fenced `text` master prompt and resolves the variables locally.
5. The frontend verifies that no unresolved placeholders remain.
6. The frontend displays the resolved prompt and keeps the input payload available in JSON View.

The current frontend generation flow does not depend on an Edge Function response contract. The Edge Function may remain deployed independently, but it is not called by `index.html`.

## 6. Validation Checklist

Before using the rendered prompt, verify:

- [ ] Required values `subject`, `headline`, and `ratio` are present.
- [ ] Every variable is resolved.
- [ ] Empty optional values are omitted cleanly.
- [ ] `feature_items` is rendered as a list when non-empty.
- [ ] User-provided product details and copy are preserved accurately.
- [ ] No unsupported product claims or features are introduced.
- [ ] The requested aspect ratio is retained.
- [ ] The final prompt contains no template syntax such as `{{subject}}`.

## 7. Change and Compatibility Rules

- Treat variable names in Section 2 as the interface contract with `index.html`.
- If a variable is renamed or added, inspect and update the frontend payload and backend variable resolver together.
- If the frontend payload changes, update this file and the backend consumer in the same integration task.
- Do not silently change the Edge Function response shape.
- Keep this file focused on the master prompt template. Put specialized AI roles or strategies in their own external-source files.
- After any integration change, test the complete path from frontend request through backend template loading and response rendering.
