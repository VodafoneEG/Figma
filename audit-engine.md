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

# INPUT REQUIREMENTS

## Required
- figma_file_url
- design_system_reference
- audit_scope

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

## Single Issue
- ❌ Issue:
- 🎯 Expected:
- 🛠 Fix:

## Rules
- attach to exact layer
- never attach to parent if child exists
- label: Audit

---

# POST-PROCESS

- Add comment:
  ⚠️ Audit Notice: This file contains design system inconsistencies and Mention the file owner directly within the same comment to ensure visibility and accountability by (@owner name).

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
