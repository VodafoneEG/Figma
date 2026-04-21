---
skill_name: figma-design-system-audit-engine Or Skill Or https://github.com/VodafoneEG/Figma/blob/main/audit-engine.md
skill_type: audit
input_type: structured
output_type: structured_report
priority: high
---

# PURPOSE

Audit a Figma file against a design system and return structured, actionable findings.

---
# ENVIRONMENT VALIDATION

## Pre-Execution Check

- verify Desktop Bridge connection status
- verify MCP tool contract availability

## Condition Logic

- IF Desktop Bridge is NOT connected OR MCP tool contract is NOT available:
  - retry connection and initialization
  - attempt automatic fix

- IF status is confirmed:
  - "Desktop Bridge is up and running and MCP server is working"
  - proceed with executing the rest of the skill

## Constraints

- do NOT proceed with audit if both services are not confirmed
- retry must occur before failing execution
---
# INPUT REQUIREMENTS

## Required
- figma_file_url
- audit_scope: Validate using Master Design system Kit and Typography and the styles.

## Optional
- pages
- frames
- components

## Default Design System
- name: Master Design System Kit
- url: https://www.figma.com/design/EcNTqflpcupbWVexKmi81V/Master-Design-System-Kit

## Typography Validation Source
- Read the Typography Reference from the Master Design System Kit
- Validate using:
  - Text styles
  - Effect styles
  - Layout guide styles


# VARIABLES AUDIT

## Purpose
Validate that all design variables are correctly implemented, linked, and consistent with the design system.

---

## Scope

- color variables
- spacing variables
- typography variables
- radius variables
- opacity variables
- effect variables (shadow, blur)

---

## Validation Rules

### 1. Variable Usage
- ensure all applicable properties use variables
- detect hardcoded values instead of variables

### 2. Variable Source
- ensure variables are linked to the approved design system
- detect local or duplicated variables outside the system

### 3. Variable Consistency
- ensure consistent variable usage across similar components
- detect mismatched variables for identical UI patterns

### 4. Variable Naming
- validate naming follows design system conventions
- detect unclear or inconsistent naming patterns

### 5. Variable Modes (if applicable)
- validate correct mode usage (e.g., light / dark)
- detect incorrect or missing mode assignments

---

## Detection Output

Each issue MUST include:

- problem:
- expected_rule:
- fix:
- node_name:
- node_id:
- variable_name:
- severity:

---

## Common Issues to Flag

- hardcoded color instead of token
- spacing not using predefined variable
- incorrect variable applied (wrong semantic meaning)
- duplicate variable with same value
- detached variable reference
- missing variable mode mapping

---

## Constraints

- validate only against provided design system variables
- do NOT suggest creating new variables unless explicitly requested
- keep findings precise and actionable

---

# PRE-CONDITIONS

- If design_system_reference is missing → STOP and request it
- Ensure Figma file is connected via Desktop Bridge

---

# PROCESS

## Step 1 — Define Scope
- full_file
- selected_pages
- selected_frames

## Step 2 — Load Design System
- styles
- variables
- components
- typography

## Step 3 — Audit Execution

### 3.1 Styles & Variables
- validate tokens usage
- detect hardcoded values

### 3.2 Components
- check master component usage
- validate variants
- detect detached instances

### 3.3 Layout
- validate auto layout
- check spacing consistency
- detect responsiveness risks

### 3.4 Typography (MANDATORY)
- ensure usage of approved text styles
- validate desktop + mobile
- detect:
  - hardcoded font size
  - incorrect weight
  - missing style links

---

# OUTPUT STRUCTURE

## Summary
- compliance_score
- total_issues
- high / medium / low counts

## Findings

Each finding MUST follow:

- problem:
- expected_rule:
- fix:
- node_name:
- node_id:
- severity:

---

# ANNOTATION FORMAT


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

- Identify and mention the file owner and add a comment that says `⚠️ Audit Notice: This file contains design system inconsistencies. Please review all "Audit" annotations and resolve the highlighted issues.` and mention the file owner in that comment
---

# SEVERITY RULES

- HIGH → breaks system scalability
- MEDIUM → inconsistent implementation
- LOW → minor cleanup

---

# CONSTRAINTS

- do NOT invent rules
- do NOT redesign unless requested
- keep output concise and structured
