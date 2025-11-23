---
description: Analyze and visualize architecture of a codebase area
argument-hint: [directory-or-area]
allowed-tools: Glob, Grep, Read, Bash(python:*)
---

# Architecture Analysis

Analyze the architecture of: **$ARGUMENTS**

## Your Task

1. **Explore** the codebase area using Glob and Read to understand:
   - Key components and their responsibilities
   - Data flow and interactions
   - Dependencies and relationships

2. **Explain** the architecture concisely:
   - Main components and their roles
   - How they interact
   - Key patterns or design decisions

3. **Visualize** by generating diagram(s) using the gemini-imagen skill scripts

## Critical Visualization Guidelines

**NEVER be vague in image prompts. The image model cannot see the codebase.**

1. **Specify exact positions**: "Component A at top center, Component B at bottom left" (not "components arranged logically")
2. **Label every connection**: "Arrow from A to B labeled 'POST /api/login with JWT'" (not "A calls B")
3. **Include all details**: Methods, parameters, return types, HTTP verbs, data formats
4. **Use specific colors**: "Requests in blue, responses in green, errors in red" (not "color-coded")
5. **State cardinality**: "1 to N (many)" on relationship lines (not "has many")
6. **Complete flows**: List every step sequentially with explicit labels

## Diagram Templates

Choose the appropriate template based on what you discovered:

### Architecture Diagram
```
"Technical architecture diagram: [COMPONENT_1] at top center, [COMPONENT_2] on left middle, [COMPONENT_3] on right middle.
Arrow from [COMPONENT_1] to [COMPONENT_2] labeled '[HTTP_METHOD] [PATH] [PURPOSE]'.
Arrow from [COMPONENT_2] to [COMPONENT_3] labeled '[PROTOCOL] [DATA_TYPE]'.
[Repeat for ALL connections with explicit labels].
Clean labeled boxes, directional arrows, white background."
```

### Data Flow Diagram
```
"Data flow diagram: Step 1: [ENTITY_A] at left. Step 2: Arrow to [ENTITY_B] labeled '[METHOD] [PATH] with [DATA]'.
Step 3: Arrow back labeled '[STATUS] [RESPONSE_TYPE]'. [Continue for all steps].
Number each step, color-code: [TYPE_1] in blue, [TYPE_2] in green. Technical style, 16:9."
```

### Component Relationships (UML)
```
"UML class diagram: [CLASS_1] box at top with attributes '[ATTRS]' and methods '[METHODS]'.
[CLASS_2] box at bottom with '[ATTRS/METHODS]'.
[CLASS_1] to [CLASS_2]: [RELATIONSHIP] shown with [ARROW_TYPE], labeled '1 to N'.
[Repeat for all relationships]. Clean UML style."
```

### Code Execution Flow
```
"Flowchart for [FUNCTION]: Start. Step 1: '[ACTION]' in blue rectangle.
Step 2: Diamond '[CONDITION]' with YES arrow to [NEXT] and NO arrow to [ALT].
[Continue all steps]. Errors in red rounded boxes, success in green. Label all arrows."
```

### Database Schema
```
"Database schema: [TABLE_1] with columns '[COL] [TYPE] [CONSTRAINTS]'.
[TABLE_2] with '[COLUMNS]'. Foreign key: [TABLE_2].[FK] → [TABLE_1].[PK]
shown with line labeled '1 to N'. [Repeat for all tables]. Show PK icons."
```

### API Endpoints
```
"REST API for [SERVICE]: Endpoint 1: [METHOD] [PATH] with body {[FIELDS]} returns {[RESPONSE]} [STATUS].
[Repeat for all endpoints]. Color-code: GET blue, POST green, PUT yellow, DELETE red.
Show full JSON examples."
```

## Educational Approach

**Your goal: Create the best visualization for understanding, not just documentation.**

Consider creative formats when appropriate:
- **Metaphors**: Database transaction as restaurant order system
- **Comics**: Function execution as sequential panels
- **Real-world scenarios**: Authentication as bouncer checking IDs
- **Analogies**: Cache as kitchen pantry with frequently-used items

**Example creative prompt:**
> "Comic strip showing JWT auth: Panel 1: User (detective) at API Gateway (security desk). Panel 2: Gateway calls Auth Service (background check). Panel 3: Auth returns golden badge (JWT). Panel 4: User shows badge to Resource Server (VIP room). Cartoon style."

## Output

- Generate diagram using: `scripts/generate_image.py "YOUR_EXPLICIT_PROMPT" architecture-diagram.png --size 4K --aspect 16:9`
- If complex, create multiple focused diagrams rather than one overwhelming image
- Save as descriptive filenames: `component-relationships.png`, `data-flow.png`, etc.
