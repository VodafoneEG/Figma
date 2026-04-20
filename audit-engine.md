# Figma Design System Audit Engine

## Description
A structured auditing system that analyzes Figma files against a published design system. It validates consistency in styles, variables, spacing, and component usage, while ensuring all UI elements adhere to master component variants. The system outputs a detailed audit report and generates actionable annotations directly tied to design issues for efficient fixing.

---

## Prompt (TC-EBC Structured)

### TASK
Conduct a comprehensive audit of a Figma file using a defined design system library.

---

### CONTEXT
You will receive a Figma file URL as input.  
The file should be audited against a **published design system library** that defines all approved styles, variables, components, and layout rules.  

This audit is intended to ensure **design consistency, scalability, and system compliance** across all screens.

---

### ELEMENTS

#### 1. Design System Compliance Audit
- Verify that all styles (colors, typography, spacing, padding) are correctly applied from the design system  
- Check usage of **design tokens / variables**  
- Identify any **hardcoded values**  

#### 2. Component & Variant Audit
- Ensure all UI elements use **master components**  
- Validate correct **variants**  
- Flag **detached components or overrides**  

#### 3. Layout & Spacing Audit
- Validate spacing consistency  
- Ensure proper layout rules (grid, auto layout, constraints)  

---

### ANNOTATION WRITING

- Generate **Figma-ready annotations** for each issue  

**Format:**
- ❌ Issue: [Incorrect element/style]  
- 🎯 Expected: [Correct design system reference]  
- 🛠 Fix: [Exact action]  

---

### FIGMA API INTEGRATION

- Use the Figma API to access file data  

Capabilities:
- Read nodes, styles, variables, components  
- Detect inconsistencies  
- Map issues to node IDs  

Assumptions:
- Full **read access**  
- Include **node name / ID** for traceability  

---

### BEHAVIOR

- Output structured audit report:
  - Compliance score  
  - Categorized issues  
  - Severity levels  

- Each issue must include:
  - Problem  
  - Correct rule  
  - Fix  
  - Annotation  

---

### CONSTRAINTS

- Only audit against provided design system  
- No redesign suggestions  
- Keep output clear and actionable  

---

### INPUT FORMAT

Figma URL: [INSERT FILE URL]  
Design System Library: [INSERT LIBRARY]  
Audit Scope: [Full / Partial]  
