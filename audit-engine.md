---
name: figma-design-system-audit-engine
description: A structured auditing system that analyzes Figma files against a published design system. It validates consistency in styles, variables, spacing, and component usage, while ensuring all UI elements adhere to master component variants. The system outputs a detailed audit report and generates actionable annotations directly tied to design issues for efficient fixing.
---

# Figma Design System Audit Engine

Use this skill when the user asks to review a Figma file for design system compliance, component correctness, variable usage, spacing consistency, or annotation-ready design QA.



## Goals

- Audit a connected Figma file against a published design system
- Identify non-compliant styles, variables, components, and layout usage
- Produce actionable findings tied to node names and node IDs
- Generate concise annotation text that can be added back into Figma

## Required Inputs

- Figma file URL or an active connected Figma file
- Design system library reference or published library source
- Audit scope: full file or a defined subset

If the design system source is missing, pause and ask for it before making compliance claims.

Default design system reference for this skill:

- Library name: `Master Design System Kit`
- Library URL: `https://www.figma.com/design/EcNTqflpcupbWVexKmi81V/Master-Design-System-Kit?m=auto&node-id=0-1&t=NnbHGAGe5ZEXVr2r-1`

Use this library as the default audit baseline unless the user explicitly provides a different design system source.

## Workflow

1. Confirm the target Figma file is connected through the Desktop Bridge plugin.
2. Identify the audit scope:
   - full file
   - selected pages
   - selected frames or components
3. Identify the design system source:
   - published library
   - shared variables
   - approved component sets
   - style definitions
4. Inspect the target file and compare it against the design system.
5. Group findings into these categories:
   - styles and variables
   - components and variants
   - detached instances and risky overrides
   - spacing and layout rules
6. For each issue, capture:
   - node name
   - node ID
   - issue summary
   - expected design system rule
   - exact fix action
   - severity
7. Return a concise audit report and, when requested, annotation-ready issue text.
8. If the user asks to apply annotations in Figma, write the findings back to the audited node using Figma annotations. Prefer `append` mode unless the user explicitly asks to replace existing notes.

## Audit Areas

### 1. Design System Compliance Audit

- Verify that colors, typography, spacing, and padding use approved styles or variables
- Check whether tokens and variables are correctly applied
- Flag hardcoded values where the design system expects reusable tokens

### 2. Component and Variant Audit

- Ensure UI elements use master components where applicable
- Validate that the chosen variant matches the approved component pattern
- Flag detached components, inconsistent substitutions, and high-risk overrides

### 3. Layout and Spacing Audit

- Check spacing consistency between similar patterns
- Review auto layout usage, constraints, alignment, and grid behavior
- Flag layout structures that are likely to break responsiveness or reuse

## Typography Audit Requirement

When auditing typography, ensure that all text elements strictly use approved text styles from the official design system:

- Typography design system: `https://www.figma.com/design/Se3vxg9JViEinpziibSK2E/Old-Design-System?m=auto&node-id=1-9&t=7DvnwbvsNJWMkHWB-1`

Validate typography usage across both Desktop and Mobile layouts.

Flag any instances of:

- hardcoded font sizes, font weights, or line heights
- text styles not linked to the design system
- inconsistent use of predefined typography tokens or styles

Any deviation should be reported and annotated with the correct style reference from the design system.

## Severity Guidance

- High: breaks design system compliance in a way that affects scalability, correctness, or shared UI consistency
- Medium: inconsistent implementation that should be normalized before handoff or release
- Low: minor deviation or cleanup item that does not materially block usage

## Output Requirements

Always provide:

- overall compliance summary
- categorized findings
- severity for each finding
- node name and node ID for traceability
- a concrete fix action

Keep findings actionable. Do not suggest redesigns unless the user explicitly asks for design alternatives.

## Finding Format

Use this structure for each issue:

- Problem: [what is wrong]
- Expected rule: [correct design system reference]
- Fix: [exact action to take]
- Node: [node name] ([node ID])
- Severity: [High / Medium / Low]

## Annotation Format

When the user asks for Figma-ready annotations, use this exact style:

- ❌ Issue: [incorrect element or style]
- 🎯 Expected: [correct design system reference]
- 🛠 Fix: [exact action]

When the user asks to place annotations directly in Figma:

- attach them to the audited frame, component, or target node
- preserve existing notes by using append mode unless replacement is requested
- keep each annotation short and traceable to one concrete issue

## Annotations

Follow these enhanced annotation requirements during audits:

- All annotations must use the label `Audit` to clearly distinguish them from other comments or notes
- Each annotation must be directly linked to the relevant design element, such as the exact frame, component, text layer, or item where the issue exists
- Ensure precise attachment to the exact layer with the issue so designers can identify and fix the problem without searching
- Prefer the most specific layer available, not the parent container
- For text-related issues, attach the annotation directly to the affected text node
- For button, icon, or component issues, attach the annotation directly to that instance or layer
- Use the parent frame only for summary notices or file-level follow-up, not for element-specific issues
- Write annotations so they are context-aware, clear, actionable, and easy to understand at a glance
- This exact-layer attachment rule is mandatory for every audit by default
- Never place an element-specific issue on a parent frame when the exact child layer is available
- If multiple layers have separate issues, annotate each layer individually instead of combining them into one parent-level note

### Annotation Styling

- The annotation label must always be `Audit`
- The desired visual treatment is a red `Audit` label for high visibility
- If the Figma annotation API does not expose label background color controls, keep the label text as `Audit` and use strong warning-oriented annotation content instead of inventing unsupported styling behavior
- Do not claim a red background was applied unless the tool actually supports and confirms it

After completing the audit and generating all annotations:

- identify and mention the file owner
- add a comment that says `⚠️ Audit Notice: This file contains design system inconsistencies. Please review all "Audit" annotations and resolve the highlighted issues.`
- mention the file owner in that comment

## Constraints

- Audit only against the provided design system
- Do not invent design system rules
- Do not provide redesign suggestions by default
- Keep results concise, traceable, and suitable for designer handoff
